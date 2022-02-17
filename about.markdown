---
layout: single
title: About
permalink: /about/
---

NuttX is a mature, free-for commercial-use, RTOS that started way back in 2007. It provides a complete framework (including peripheral APIs) that is standardised across the many platforms it supports. It has strict adherence to posix standards so should be easy to pick up. It has all the connectivity options you would find in a modern framework, suitable to be called a IoT operating system.

An excellent documentation is available at the official [website](https://nuttx.apache.org/docs/latest/index.html). I will try to complement these docs with practical use-cases and tips here.

The RTOS has good support for RISC-V chips, and mainly for the cheap chips/boards that can be purchased through outlets such as Aliexpress. I'm a big fan of these cheap low cost dev boards.

Good resources about RTOS theory include:
- Digi-key [Youtube series](https://www.youtube.com/watch?v=F321087yYy4)
- μC/OS-II User's Manual Chapter 2: "Real-Time Systems Concepts" pp47. Accessible [here](http://www.farnell.com/datasheets/1950186.pdf) is an excellent intro into the world of RTOS. It has slow paced, thorough treatment.

Before I discovered NuttX though, I picked up on the linux based RTOS - Zephyr[^1]. I made a write-up on the process and it is available at [Zephyr-guide](https://simplycreate.online/zephyr-guide/). Since I discovered NuttX I abandoned it, and only covers the very basics.


[^1]: https://www.zephyrproject.org/
