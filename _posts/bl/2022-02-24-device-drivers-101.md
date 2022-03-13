---
layout: single
title:  "Device Drivers 101"
date:   2022-02-24 02:27:43 +1100
categories: bl702

---

> The SDK has the provisions for [device tree system](https://elinux.org/Device_Tree_Reference). However this has been segregated into the flash tool with all the communication ports enabled. We will write our device driver in the conventional way in pure C, built on top of the relevant peripheral API; eg, UART, I2C...


## SSD1306 OLED Display

The SSD1306 128x64 oled display is a popular I/O device that can be added to any project for example to provide some visual diagnostics. It comes in two main variaties; the I2C and the SPI version. We'll be using the I2C one.

![SSD1306 128x64](/assets/oled.jpg)


Fortunately the SDK has already the implementation for the lvgl graphics library (provides APIs to simplify access to the display), so we just need to write the I2C protocol driver for oled. This will be an exercise in writing device drivers; first of many more to come!

