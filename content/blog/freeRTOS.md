+++
title = "freeRTOS Overview"
date = 2022-02-24
draft = false

[taxonomies]
categories = ["bl702"]
tags = ["bl702", "sdk", "freeRTOS"]

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

The SDK includes the freeRTOS as the system kernel. More info can be seen by clicking on the 'More Info' button on the top of this page. But the kernel is already configured for use in the SDK, so we do not need to worry about it's config or setting up.

In this article we'll go through a rough overview of freeRTOS of just the necessary features.

![2tasks](/img/2tasks.gif)

## Tasks

```

#include <hosal_gpio.h>
#include <FreeRTOS.h>
#include <task.h>

static hosal_gpio_dev_t gp1, gp2;

void my_task (void *p)
{
   static _Bool val = 1;

   while (1) {
      hosal_gpio_output_set(&gp2, val);
      val = !val;
      vTaskDelay(pdMS_TO_TICKS(700));
   }
}

void main (void)
{
   // setup
   gp1.port = 5;
   gp1.config = OUTPUT_OPEN_DRAIN_NO_PULL;
   hosal_gpio_init(&gp1);
   _Bool value = 1;
   // setup second gpio
   gp2.port = 4;
   gp2.config = OUTPUT_OPEN_DRAIN_NO_PULL;
   hosal_gpio_init(&gp2);

   xTaskCreate(my_task, "second_entry", 1024, NULL, 15, NULL);

   while (1) {	// should never reach this point
      hosal_gpio_output_set(&gp1, value );
      value = !value;
      vTaskDelay(pdMS_TO_TICKS(500));
   }
}
```

Tasks are created using the function in line 30. You need to specify the entry callback function here (as first parameter) and have it defined. The external task we define here is ```my_task``` in line 7. Other parameters are as follows:

- ```"second_entry"```: A descriptive name for the task. This is not used by FreeRTOS in any
way. It is included purely as a debugging aid.

- ```1024```: The stack depth. Note the value is ```number-of-bytes``` / 4. Learning to choose the right size is an artform you pickup as you work through the examples.

- ```NULL```: Parameter to our callback function. Here we're passing nothing.

- ```15```: The task Priority. See below.

- ```Null```: Not used.

In this example we maintain our blinky program in main that toggles every 500ms. In addition to this we are creating a task for a secondary blinky on port D4, with a different frequency... This example shows how an off-the self kernel eases our multitasking.

### Priorities

The SDK sets the maximum number of priorities to 32. See configMAX_PRIORITIES in ```bl_iot_sdk/components/platform/soc/bl602/freertos_riscv/config/FreeRTOSConfig.h```.

Higher the value: higher the priority. If two tasks have the same priority a round robin scheduling is done.
To modify the priority of a running task use ```vTaskPrioritySet(n)```.

## Queues

## Semaphores

### Mutex
