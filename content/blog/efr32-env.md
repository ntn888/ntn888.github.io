+++
title = "Setting up the environment"
date = 2025-02-19 02:24:00
draft = false

[taxonomies]
categories = ["efr32"]
tags = ["efr32", "Silicon Labs", "low-power wireless", "sdk"]

[extra]
lang = "en"
toc = true
comment = false
copy = true
math = false
mermaid = false
outdate_alert = false
outdate_alert_days = 120
display_tags = true
truncate_summary = false
+++

In this article we will see about setting up a development environment for Silicon Labs' BLE/Thread range of MCUs. Specifically for the EFR32MG21A020F768IM32 chip which appears in the inexpensive ZYZBP008 module. I built a breakout board for this module. KiCAD files available [here](https://github.com/ntn888/efr32_breakout).

Silicon labs' EFR32 series IOT low power wireless chips are low cost and have good availability. They're know for good power efficiency and being of western origin; have great documentation. Another attractive feature (unlike Nordic) is the traditional embedded development SDK.

Like ST they provide an Eclipse based IDE. But this is resource heavy to run on the host, and has been laggy under Linux (for me). As usual in this blog we avoid propriety IDEs and have a generic VIM/makefile workflow.

Producing makefiles by hand has another inadvertent advantage: you intricately learn the project dependencies. Which helps you understand the environment better.

First install dependencies:

```
sudo apt install git git-lfs gcc-arm-none-eabi make bear gcc-multilib libstdc++-arm-none-eabi-newlib openocd gdb-multiarch
```
On arch:

```
sudo pacman -Sy git git-lfs make bear arm-none-eabi-gcc arm-none-eabi-newlib arm-none-eabi-gdb openocd
```

Then clone the gecko SDK:

```
git clone https://github.com/SiliconLabs/gecko_sdk.git 
```

Alternatively download the SDK zip file from their releases. This results in a smaller download.

Now CD into this directory. We now need to create the files for a makefile structured project. The SDK uses a `SLC` based projects, but we will ignore this. I only succeeded to generate the blinky example using this. We will use makefile instead. This is the resulting file tree:

```
gecko-sdk
|
├── platform
│    ├── CMSIS
│    └── emlib
│    │
│    ...
│
└── myapps
     └── blinky
          ├── Makefile
          ├── main.mk
          ├── emlib.mk
          ├── compile_commands.json
          ├── linker_script
          │     └── efr32mg21.ld
          └── src
               └── main.c
```

We see the familiar `CMSIS` directory. `emlib` is Silicon labs' equivalent of peripheral HAL. The main make `Makefile` is a template available [here](https://github.com/arturlangner/EFM32_makefile_project/blob/master/Makefile). I copied the linker script over from `gecko-sdk/platform/Device/SiliconLabs/EFR32MG21/Source/GCC/efr32mg21.ld`. Note that this file differs between projects, especially if an RTOS is used.

`main.mk`:

```
CC = arm-none-eabi-gcc
SIZE = arm-none-eabi-size
OBJCOPY = arm-none-eabi-objcopy

COMMON_FLAGS = \
 -mcpu=cortex-m33 \
 -mthumb \
 -mfpu=fpv5-sp-d16 \
 -mfloat-abi=hard \
 -std=c99 \
 -Wall \
 -Wextra \
 -Os \
 -fdata-sections \
 -ffunction-sections \
 -fomit-frame-pointer \
 -imacros sl_gcc_preinclude.h \
 -mcmse \
 -g

DEFINES = -DEFR32MG21A020F768IM32

INCLUDE += -I include -I ../../platform/CMSIS/Core/Include -I ./ \
 -I../../platform/Device/SiliconLabs/EFR32MG21/Include \
 -I../../platform/common/inc \
 -I ../../platform/common/toolchain/inc \
 -I ../../platform/emlib/inc


CFLAGS := $(COMMON_FLAGS) $(INCLUDE) $(DEFINES)

SUBMAKEFILES := emlib.mk
SOURCES := ../../platform/Device/SiliconLabs/EFR32MG21/Source/system_efr32mg21.c \
../../platform/Device/SiliconLabs/EFR32MG21/Source/startup_efr32mg21.c \
src/main.c


BUILD_DIR  := build
TARGET_DIR := build_output

#nosys.specs are required for printf
LDFLAGS =  \
 -mcpu=cortex-m33 \
 -mthumb \
 -mfpu=fpv5-sp-d16 \
 -mfloat-abi=hard \
 -T"linker_script/efr32mg21.ld" \
 --specs=nano.specs \
 #--specs=nosys.specs \
 -Xlinker -Map=$(TARGET_DIR)/main.map \
 -Wl,--gc-sections
 
# Startup file
#LDLIBS := ../../platform/Device/SiliconLabs/EFR32MG21/Source/GCC/startup_efr32mg21.S

###########

GROUP_START =-Wl,--start-group
GROUP_END =-Wl,--end-group

PROJECT_LIBS = \
 -lgcc \
 -lc \
 -lm \
 -lnosys

LIBS += $(GROUP_START) $(PROJECT_LIBS) $(GROUP_END)

TGT_LDFLAGS += $(LIBS)
##########

TARGET := main.elf

size: $(TARGET_DIR)/main.elf
	$(SIZE) $(TARGET_DIR)/main.elf

```

`emlib.mk`

```
SOURCES := ../../platform/emlib/src/em_cmu_fpga.c \
../../platform/emlib/src/em_dma.c \
../../platform/emlib/src/em_csen.c \
../../platform/emlib/src/em_msc.c \
../../platform/emlib/src/em_lcd.c \
../../platform/emlib/src/em_rtcc.c \
../../platform/emlib/src/em_can.c \
../../platform/emlib/src/em_wdog.c \
../../platform/emlib/src/em_system.c \
../../platform/emlib/src/em_prs.c \
../../platform/emlib/src/em_se.c \
../../platform/emlib/src/em_eusart.c \
../../platform/emlib/src/em_dbg.c \
../../platform/emlib/src/em_cmu.c \
../../platform/emlib/src/em_rmu.c \
../../platform/emlib/src/em_vcmp.c \
../../platform/emlib/src/em_gpio.c \
../../platform/emlib/src/em_aes.c \
../../platform/emlib/src/em_burtc.c \
../../platform/emlib/src/em_usart.c \
../../platform/emlib/src/em_qspi.c \
../../platform/emlib/src/em_lesense.c \
../../platform/emlib/src/em_adc.c \
../../platform/emlib/src/em_core.c \
../../platform/emlib/src/em_letimer.c \
../../platform/emlib/src/em_idac.c \
../../platform/emlib/src/em_ldma.c \
../../platform/emlib/src/em_vdac.c \
../../platform/emlib/src/em_rtc.c \
../../platform/emlib/src/em_dac.c \
../../platform/emlib/src/em_pcnt.c \
../../platform/emlib/src/em_pdm.c \
../../platform/emlib/src/em_cryotimer.c \
../../platform/emlib/src/em_iadc.c \
../../platform/emlib/src/em_acmp.c \
../../platform/emlib/src/em_opamp.c \
../../platform/emlib/src/em_timer.c \
../../platform/emlib/src/em_i2c.c \
../../platform/emlib/src/em_gpcrc.c \
../../platform/emlib/src/em_crypto.c \
../../platform/emlib/src/em_emu.c \
../../platform/emlib/src/em_ebi.c \
../../platform/emlib/src/em_leuart.c
```

`main.c`:

```
#include "em_cmu.h"
#include "em_gpio.h"

int main (void)
{
  CMU_ClockEnable(cmuClock_GPIO, true);
  GPIO_PinModeSet(gpioPortA, 0/*pin 0*/, gpioModePushPull /*push-pull output*/, 1/*output level*/);
  
  while(1) {
    GPIO_PinOutToggle(gpioPortA, 0/*pin 0*/);
    for (volatile uint32_t i = 0; i < 100000; i++) { } // busy delay
  }
  
  return 0;
}

```

You can run `make -j` to build. To get intellisense in VIM/VSCODE we need to produce the `compile_commands.json` file. To do this make sure to clean the build `rm -rf build build_output` and run ` bear -- make -j`.
