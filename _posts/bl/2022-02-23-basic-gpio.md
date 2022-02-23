---
layout: single
title:  "Basic GPIO BL602"
date:   2022-02-23 11:27:43 +1100
categories: bl602

---

The BL602 chip has 22 GPIOs. Each pin can be selected as one of the following modes:


- ANALOG_MODE

    Used as a function pin, input and output analog.

- INPUT_PULL_UP

    Input with an internal pull-up resistor - use with devices that actively drive the signal low - e.g. button connected to ground.

- INPUT_PULL_DOWN

    Input with an internal pull-down resistor - use with devices that actively drive the signal high - e.g. button connected to a power rail.

- INPUT_HIGH_IMPEDANCE

    Input - must always be driven, either actively or by an external pullup resistor.

- OUTPUT_PUSH_PULL

    Output actively driven high and actively driven low - must not be connected to other active outputs - e.g. LED output.

- OUTPUT_OPEN_DRAIN_NO_PULL

    Output actively driven low but is high-impedance when set high - can be connected to other open-drain/open-collector outputs. Needs an external pull-up resistor.

- OUTPUT_OPEN_DRAIN_PULL_UP

    Output actively driven low and is pulled high with an internal resistor when set high - can be connected to other open-drain/open-collector outputs.

- OUTPUT_OPEN_DRAIN_AF

    Alternate Function Open Drain Mode.

- OUTPUT_PUSH_PULL_AF

    Alternate Function Push Pull Mode.


## Using GPIO

The relevant functions are defined in ```hosal_gpio.h```. Include this header before using any GPIO functions. An example application is as follows:

{% highlight c linenos %}
#include <stdio.h>
#include <hosal_gpio.h>
#include <FreeRTOS.h>
#include <task.h>

static hosal_gpio_dev_t gp1;

void main (void)
{
   // setup
   gp1.port = 5;
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

Before we could use the GPIO API, the GPIO struct must be defined and assigned as in line6. Here we're naming it as ```gp1``` to reference later.

Then we must initialise and config a GPIO pin before we can use it. This tas is done in lines 11-13.

The possible value for the 'config' setting was introduced in the start of this article; see above. Finally call the function ```hosal_gpio_init```. Now we can use the API to control them.

As basic functionality of GPIOs, we set the output levels using the following commands:
```
hosal_gpio_output_set(hosal_gpio_dev_t *gpio, uint8_t value)
```
Value 0 sets the pin in logical 0 / inactive state. Value 1 sets the pin in logical 1 / active state.

To read an input:
```
hosal_gpio_input_get(hosal_gpio_dev_t *gpio, uint8_t *value)
```

I've looked for a GPIO 'toggle' function to simplify this example, but unfortunately the API docs do not indicate any...

## Interrupt Mode

< TODO >

## ALT Function Mode

< TODO >
