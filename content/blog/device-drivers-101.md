+++
title = "Device Drivers 101"
date = 2022-02-24 00:31:00
draft = false

[taxonomies]
categories = ["bl702"]
tags = ["bl702", "sdk", "device drivers"]

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

> The SDK has the provisions for [device tree system](https://elinux.org/Device_Tree_Reference). However this has been segregated into the flash tool with all the communication ports enabled. We will write our device driver in the conventional way in pure C, built on top of the relevant peripheral API; eg, UART, I2C...

![i2c gpio extender](/img/PCF8574.jpeg)

## I2C GPIO extender

To exercise the I2C bus in this post, we’ll be using the PCF8574 GPIO extender chip. This is a great chip for adding additional GPIO lines, provided that you don’t need high speed (the demo operates the I2C bus at ??? kHz). This will be an exercise in writing device drivers; first of many more to come!

