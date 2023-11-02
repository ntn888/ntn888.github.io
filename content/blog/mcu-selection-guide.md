+++
title = "MCU Selection Guide"
date = 2022-03-16
draft = false

[taxonomies]
categories = ["misc"]
tags = ["fimware develompent", "embedded hobbiest", "microcontrollers"]

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

There are a multitude of chip manufacturers, let alone individual chip modules. It can get overwhelming quickly for a newbie in the field of embedded or IOT systems, especially if all you want to do is tinker at a hobbiest level.

I present to you a selection guide based on 3 modules to serve a wide range of applications. The rationale being, by having only a few select chips in your toolset you can put the effort to master your chips and get into the nitty gritty. This leads to less frustration when developing and also by working on the native toolsets for the chip in hand, you're confident in producing the most efficient code.

With this in mind I present to you the following list:

- ~~Bluepill (stm32f103c8)~~[^1]: sub-dollar chip that's simple and very compact formfactor. Deploy it for the simplest of applications. You can still run freeRTOS! And even drive an OLED display! An excellent intro using free licence tools are covered in the book: [Beginning STM32](https://link.springer.com/book/10.1007/978-1-4842-3624-6) by Warren Gay.

- XT-ZB1 (bl702): the king of versatility! Although intended for IOT applications, the $2 price tag opens the possibilities to deploy into anything! A RISC-V clocked at 144 MHz including FPU. That’s decent enough for most of demanding applications.

- DT-BL10 (bl602): if you absolutely must implement WiFi on a microcontroller, this board is your friend. I hear it runs with better power efficiency than an ESP chip. But even still saves you from needing to learning another SDK.

Between the Bluepill and the BL702, I believe you have the world of embedded at your hands!

[^1]:Edit: See [this post](https://simplycreate.online/update/2022/11/04/pico_4m.html) about the PI Pico variant. This would be an excellent replacement for the bluepill. With 4M flash and 130MHz M0+ processor it is a no brainer for new projects!!
