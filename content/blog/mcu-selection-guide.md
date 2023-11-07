+++
title = "Low-cost microcontrollers for hobbyists"
date = 2022-03-16
draft = false

[taxonomies]
categories = ["misc"]
tags = ["fimware develompent", "embedded hobbyist", "microcontrollers"]

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

With this in mind I present to you the following list[^2]:

- ~~Bluepill (stm32f103c8)~~[^1]: sub-dollar chip that's simple and very compact formfactor. Deploy it for the simplest of applications. You can still run freeRTOS! And even drive an OLED display! An excellent intro using free licence tools are covered in the book: [Beginning STM32](https://link.springer.com/book/10.1007/978-1-4842-3624-6) by Warren Gay.

- Micro:Bit (nRF52833): low power wireless! When it comes to low power wireless, Nordic is the undisputed king. RF is in their name and they have gained good popularity in the industry over the past years. See [this post](@/blog//micro-bit-breakout.md) for viable hobbyist boards.

- XT-ZB1 (bl702): the king of versatility! Although intended for IOT applications, the $2 price tag opens the possibilities to deploy into anything! A RISC-V clocked at 144 MHz including FPU. That’s decent enough for most of demanding applications. Note that the fact the SDK is still in developement (after 2 years of release!) and that it's unsupported by any IOT RTOS frameworks (RT-Thread for example) makes is a questionable choice right now and we go with the above chip for wireless.


Between the Bluepill and the nRF52, I believe you have the world of embedded at your hands!

[^1]:Edit: See [this post](@/blog/pico_4m.md) about the PI Pico variant. This would be an excellent replacement for the bluepill. With 4M flash and 130MHz M0+ processor it is a no brainer for new projects!!

[^2]: If you're interested in beginner boards or controller see [this post](@/blog/begin-embedded.md).
