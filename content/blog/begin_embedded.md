+++
title = "Best microcontroller for Beginners"
date = 2023-11-05 18:27:00
draft = false

[taxonomies]
categories = ["misc"]
tags = ["stm32", "blue-pill", "embedded hobbiest", "microcontrollers"]

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

One of the most asked questions on r/embedded Reddit is "How do I get started in embedded electronics?" or "What is the best microcontroller for beginners?". This post will aim to address that.

If you've seen my *About* page in this blog, I've discussed a possible set of microcontrollers to keep in his/her skillset as a hobbyiest. Here we will elaborate.

There are wide range of microcontrollers, here are some popular microcontrollers that are great choices for beginners:

1. **Arduino:** Arduino is widely considered one of the best options for beginners. It offers a user-friendly, open-source platform with a simple programming environment. The Arduino community is large and supportive, and there are numerous tutorials and projects available online. The Arduino Uno and Arduino Nano are excellent starter boards.

2. **Raspberry Pi Pico:** The Raspberry Pi Pico is a microcontroller board from the Raspberry Pi Foundation. It's based on the RP2040 microcontroller and is excellent for beginners. It supports the MicroPython programming language, which is easy to learn and use. The Pico is affordable and has plenty of online resources.

3. **Mbed platform:** Then there is also the Mbed platform, developed by Arm, a solid choice for those who want to get into more professional-grade microcontroller programming. Mbed provides a free online IDE and supports a wide range of development boards.

4. **Micro:bit:** The BBC Micro:bit is designed for education and is an excellent choice for beginners, especially in a classroom setting. It has a user-friendly block-based programming environment but can also be programmed in Python and JavaScript. It's ideal for teaching programming and electronics concepts.

Many take the Arduino route. Although this may be a good idea; in this blog we're not concerned with Arduino; these are heavy frameworks. Simple, plain and close to metal frameworks are more performant and more importantly - exciting.

Hence my suggestion is to begin with the widely available and inexpensive [blue-pill board](https://web.archive.org/web/20190428082446/http://wiki.stm32duino.com/index.php?title=Blue_Pill). It is powered by the ST's ARM Cortex-M3 stm32f103c8; A chip that is widely used in the industry. It's got enough RAM and flash to run your initial projects while you pick up some embedded skills.

Pairing it up with this book: [Beginning STM32](https://www.amazon.com/BEGINNING-STM32-DEVELOPING-LIBOPENCM3-Paperback/dp/1484245970), will give you a great start with step-by-step hand-holding. This book uses the [libopenCM3](https://github.com/libopencm3/libopencm3) HAL (that's Hardware Abstraction Layer) framework. In my opinion this framework is far superior to the ST's default cube HAL which is a botched mess.

![Begining STM32 cover](/img/begin_stm32.jpg)

To complement your microcontroller board and to do projects, you will need a set of commonly used electronic components. Fortunately one may find many *electronic starter-kits* on AliExpress that will suffice for a beginner hobbyiest.

