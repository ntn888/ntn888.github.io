+++
title = "Basic application structure"
date = 2022-02-22
draft = false

[taxonomies]
categories = ["bl702"]
tags = ["bl702", "sdk", "environment"]

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


The ```bl_iot_sdk``` is the development toolkit provided for the development of applications for the BL70X and BL60X controllers. It is quite a pleasant environment built upon the following major components:

- freeRTOS kernel
- lwIP stack
- 'HOSAL' API (based off AliOS-Things HAL)

To acquire the SDK, use the following git command (preferably from the home folder):
```
git clone https://github.com/bouffalolab/bl_iot_sdk.git
```


> **ProTip**
>
> The sdk docs are available at [https://bouffalolab.github.io/bl_iot_sdk/index.html](https://bouffalolab.github.io/bl_iot_sdk/index.html). Use the firefox translate [plugin](https://addons.mozilla.org/en-US/firefox/addon/traduzir-paginas-web/) to view it in English. I found that the chrome version is a bit finnicky...

--------------------

A basic bl-iot-sdk project structure is as follows:
```
.
├── compile_commands.json
├── main
│   ├── bouffalo.mk
│   └── main.c
├── Makefile
└── proj_config.mk
```

One file may already be familiar to you: ```compile_commands.json```. If not see [this post](@/blog/dev-env.md) on how to setup a dev environment.


```main.c``` is the main application C file. It contains the ```main()``` function. In our blinky example:
{% highlight c linenos %}
#include <stdio.h>
#include <hosal_gpio.h>
#include <FreeRTOS.h>
#include <task.h>

static hosal_gpio_dev_t gp1;

void main (void)
{
   // setup
   gp1.port = 0; // <== make sure led connected to pin D0!
   gp1.config = OUTPUT_OPEN_DRAIN_NO_PULL;
   hosal_gpio_init(&gp1);
   hosal_gpio_output_set(&gp1, 1);

   uint8_t value = 1;

   while (1) {
      hosal_gpio_output_set(&gp1, value );
      value = !value;
      vTaskDelay(500);
   }
}  
{% endhighlight %} 
The specifics of this file are not important to us right now. We will study the contents in the next post.

The other three makefiles are simply copied over from the official blinky sample, and are required to successfully build the project.

The main makefile ```Makefile``` in the project root, is slightly modified:
```
#
# This is a project Makefile. It is assumed the directory this Makefile resides in is a
# project subdirectory.
#

PROJECT_NAME := main
BL60X_SDK_PATH=~/bl_iot_sdk
CONFIG_CHIP_NAME=BL702
export BL60X_SDK_PATH CONFIG_CHIP_NAME
PROJECT_PATH := $(abspath .)
PROJECT_BOARD := evb
export PROJECT_PATH PROJECT_BOARD
#CONFIG_TOOLPREFIX :=

-include ./proj_config.mk

ifeq ($(origin BL60X_SDK_PATH), undefined)
BL60X_SDK_PATH_GUESS ?= $(shell pwd)
BL60X_SDK_PATH ?= $(BL60X_SDK_PATH_GUESS)/../../..
$(info ****** Please SET BL60X_SDK_PATH ******)
$(info ****** Trying SDK PATH [$(BL60X_SDK_PATH)])
endif

COMPONENTS_BLSYS   := bltime blfdt blmtd bloop loopset looprt
COMPONENTS_VFS     := romfs

SOC_DRV = $(shell echo $(CONFIG_CHIP_NAME) | tr A-Z a-z)


INCLUDE_COMPONENTS += $(SOC_DRV)_rf
INCLUDE_COMPONENTS += $(SOC_DRV)_freertos

INCLUDE_COMPONENTS += $(SOC_DRV) $(SOC_DRV)_std
INCLUDE_COMPONENTS += hosal mbedtls_lts lwip cli vfs yloop utils blog blog_testc newlibc
INCLUDE_COMPONENTS += $(COMPONENTS_NETWORK)
INCLUDE_COMPONENTS += $(COMPONENTS_BLSYS)
INCLUDE_COMPONENTS += $(COMPONENTS_VFS)
INCLUDE_COMPONENTS += $(PROJECT_NAME)

include $(BL60X_SDK_PATH)/make_scripts_riscv/project.mk
```
We have hardcoded the chip name and the sdk path. This makes the make command simpler. Also note that we have changed the ```PROJECT_NAME``` to a generic name: main. This avoids the need to modify the variable to match each project.

A quick tutorial on Make is viewable [here](https://www.cs.colby.edu/maxwell/courses/tutorials/maketutor/).

NOTE: The makefile variable ```BL60X_SDK_PATH``` assumes that you have cloned the sdk into your home directory. If not please modify this variable to reflect your chosen path.

In ```proj_config.mk```:

```

####
CONFIG_SYS_VFS_ENABLE:=1
CONFIG_SYS_VFS_UART_ENABLE:=1
CONFIG_SYS_AOS_CLI_ENABLE:=1
CONFIG_SYS_AOS_LOOP_ENABLE:=1
CONFIG_SYS_BLOG_ENABLE:=1
CONFIG_SYS_DMA_ENABLE:=1
CONFIG_SYS_USER_VFS_ROMFS_ENABLE:=0
CONFIG_SYS_APP_TASK_STACK_SIZE:=4096
CONFIG_SYS_APP_TASK_PRIORITY:=15


CONFIG_SYS_COMMON_MAIN_ENABLE:=1
CONFIG_BL702_USE_ROM_DRIVER:=1
CONFIG_BUILD_ROM_CODE := 1
CONFIG_USE_XTAL32K:=1


LOG_ENABLED_COMPONENTS:= blog_testc hosal
```

In ```bouffalo.mk```:
```
#
# "main" pseudo-component makefile.
#
# (Uses default behaviour of compiling all source files in directory, adding 'include' to include path.)

include $(BL60X_SDK_PATH)/components/network/ble/ble_common.mk

ifeq ($(CONFIG_ENABLE_PSM_RAM),1)
CPPFLAGS += -DCONF_USER_ENABLE_PSRAM
endif

ifeq ($(CONFIG_ENABLE_CAMERA),1)
CPPFLAGS += -DCONF_USER_ENABLE_CAMERA
endif

ifeq ($(CONFIG_ENABLE_BLSYNC),1)
CPPFLAGS += -DCONF_USER_ENABLE_BLSYNC
endif

ifeq ($(CONFIG_ENABLE_VFS_SPI),1)
CPPFLAGS += -DCONF_USER_ENABLE_VFS_SPI
endif

ifeq ($(CONFIG_ENABLE_VFS_ROMFS),1)
CPPFLAGS += -DCONF_USER_ENABLE_VFS_ROMFS
endif

CPPFLAGS += -DCONF_USER_BL702
```

> As a convenience you may simply download the following [project skeleton](/assets/src.zip). Extract it to a 'workspace' directory of choice; then whenever you want to initiate a new project, simply run:
> ```cp -a src/ my_proj/```

Once the above files are in place all you have to do is run:
```
make  -j
```
Once your project compiles successfully; you may run ```make flash``` to burn the firmware (ensuring you have the chip in bootloader mode).

If you ever run into problems run ```make clean``` as the first step.
