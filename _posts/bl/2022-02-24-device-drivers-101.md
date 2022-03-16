---
layout: single
title:  "Device Drivers 101"
date:   2022-02-24 02:27:43 +1100
categories: bl702

---

> The SDK has the provisions for [device tree system](https://elinux.org/Device_Tree_Reference). However this has been segregated into the flash tool with all the communication ports enabled. We will write our device driver in the conventional way in pure C, built on top of the relevant peripheral API; eg, UART, I2C...

![i2c gpio extender](/assets/PCF8574.jpeg)

## I2C GPIO extender

To exercise the I2C bus in this post, we’ll be using the PCF8574 GPIO extender chip. This is a great chip for adding additional GPIO lines, provided that you don’t need high speed (the demo operates the I2C bus at ??? kHz). This will be an exercise in writing device drivers; first of many more to come!

