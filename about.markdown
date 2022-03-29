---
layout: single
title: About
permalink: /about/
---


The  BL702 is a new offering from the chip design company Bouffalo Lab, that is extremely cheap, performant RISC-V chip. With all the bells & whistles of IoT, it's got BLE, 802.15.4, and Zigbee. See [this post](https://simplycreate.online/update/2022/02/19/xt-zb1-bl702.html) for further info.

It is envisaged that as the BL702 catches on it'll get support off the many MCU frameworks maybe even Arduino. This text however will be concerned with the manufacture's vanilla sdk offering: bl-mcu-sdk. In this blog we shall take a deep-dive into this environment, learning to control peripherals through the API and also look into how to write devices drivers to connecting to external modules.
The SDK by  default uses the freeRTOS, a real-time kernel that enables multitasking; makes the life of a firmware dev easier.

Good resources about RTOS theory include:
- Digi-key [Youtube series](https://www.youtube.com/watch?v=F321087yYy4)
- μC/OS-II User's Manual Chapter 2: "Real-Time Systems Concepts" pp47. Accessible [here](http://www.farnell.com/datasheets/1950186.pdf) is an excellent intro into the world of RTOS. It has slow paced, thorough treatment.

Before I ventured into the BL chips though, I picked up on the linux based RTOS - Zephyr[^1]. I made a write-up on the process and it is available at [Zephyr-guide](https://simplycreate.online/zephyr-guide/). Since then abandoned it, and only covers the very basics.


[^1]: https://www.zephyrproject.org/
