+++
title = "Serial UART"
date = 2022-02-24 00:28:00
draft = false

[taxonomies]
categories = ["bl702"]
tags = ["bl702", "sdk", "UART"]

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

We will study the UART peripheral here. First the basic polling mode.

```
#include <stdio.h>
#include <hosal_uart.h>
#include <FreeRTOS.h>
#include <task.h>

//hosal_uart_dev_t uart_dev;

HOSAL_UART_DEV_DECL(uart_dev, 0, 16, 7, 2000000);

void main (void)
{
   char data[] = "test\n";

   // uart setup (for info)
   hosal_uart_init(&uart_dev);

   while (1) {
      hosal_uart_send(&uart_dev, data, sizeof(data));
      vTaskDelay(500);
   }
}
```

We begin by including the relevant header ```hosal_uart.h```. The initialisation is done in 2 parts. In line 8, we use the macro to configure the settings as desired, including which uart peripheral we select in the second argument, here we select the uart0.
Next we simply call in line 16.

## Interrupt Mode

< todo >
