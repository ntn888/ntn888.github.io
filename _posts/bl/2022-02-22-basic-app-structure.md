---
layout: single
title:  "Basic application structure"
date:   2022-02-22 11:27:43 +1100
categories: bl602

---

---
**ProTip**

The sdk docs are available at [https://bouffalolab.github.io/bl_iot_sdk/index.html](https://bouffalolab.github.io/bl_iot_sdk/index.html). Use the firefox translate [plugin](https://addons.mozilla.org/en-US/firefox/addon/traduzir-paginas-web/) to view it in English. I found that the chrome version is a bit finnicky...

---

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

One file may already be familiar to you: ```compile_commands.json```. If not see [this post](https://simplycreate.online/bl602/2022/02/21/dev-env.html) on how to setup a dev environment.


```main.c``` is the main application C file. It contains the ```main()``` function. In our blinky example:
```
#include <stdio.h>
#include <string.h>
#include <FreeRTOS.h>
#include <task.h>
#include <bl_gpio.h>

#define GPIO_LED_PIN 5

void blink_test(void *param)
{
    uint8_t value = 1;
    while(1) {
        bl_gpio_enable_output(GPIO_LED_PIN, 0, 0);
        bl_gpio_output_set(GPIO_LED_PIN, value);
        value = !value;
        vTaskDelay(500);
    }
}

void main(void)
{
    xTaskCreate(blink_test, "blink", 1024, NULL, 15, NULL);
}
  
```
The specifics of this file are not important to us right now. We will study the contents in the next post.

The other three makefiles are simply copied over from the official blinky sample, and are required to successfully build the project.

The main makefile ```Makefile``` in the project root, is slightly modified:
```
#
# This is a project Makefile. It is assumed the directory this Makefile resides in is a
# project subdirectory.
#

# hardcode sdk_path, chip
BL60X_SDK_PATH=~/bl_iot_sdk
CONFIG_CHIP_NAME=BL602
export BL60X_SDK_PATH CONFIG_CHIP_NAME
PROJECT_NAME := main
PROJECT_PATH := $(abspath .)
PROJECT_BOARD := evb
export PROJECT_PATH PROJECT_BOARD
#CONFIG_TOOLPREFIX :=

-include ./proj_config.mk

COMPONENTS_BLSYS   := bltime blfdt blmtd bloop loopadc looprt loopset
COMPONENTS_VFS     := romfs

INCLUDE_COMPONENTS += freertos_riscv_ram bl602 bl602_std newlibc hosal mbedtls_lts lwip vfs yloop utils cli blog blog_testc coredump
INCLUDE_COMPONENTS += $(COMPONENTS_NETWORK)
INCLUDE_COMPONENTS += $(COMPONENTS_BLSYS)
INCLUDE_COMPONENTS += $(COMPONENTS_VFS)
INCLUDE_COMPONENTS += $(PROJECT_NAME)

include $(BL60X_SDK_PATH)/make_scripts_riscv/project.mk
```
We have hardcoded the chip name and the sdk path. This makes the make command simpler. Also note that we have changed the ```PROJECT_NAME``` to a generic name: main. This avoids the need to modify the variable to match each project.

A quick tutorial on Make is viewable [here](https://www.cs.colby.edu/maxwell/courses/tutorials/maketutor/).

NOTE: The makefile variable ```BL60X_SDK_PATH``` assumes that you have cloned the sdk into your home directory. If not please modify this variable to reflect your chosen path.

Once the above files are in place all you have to do is run:
```
make CONFIG_LINK_ROM=1 -j
```
If you ever run into problems run ```make clean``` as the first step.
