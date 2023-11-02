+++
title = "C linker scripts howto"
date = 2022-11-16
draft = false

[taxonomies]
categories = ["asm"]
tags = ["assembly", "risc-v"]

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


In this article series we will look at dealing and understanding linker script writing. Targetting the RISC-V RV32 core. We begin with looking at programming assembly, as this will help understand the innards of the microcontroller.

<!-- more -->

The RISC-V chip was intended and intentionally designed as a heuristic use case, coming in from academic origins has now found popularity among the commercial setting. As such it is ideal microcontroller for tackling to understand the assembly environment, and given it's rising populatity among the other giants in the field especially ARM cortex; has practical value.

You may ofcourse run a simulation by installing the toolchain and qemu along with gdb; which then enables the ability to run in step and inspect the memory state. However for a total beginner an all inclusive ready-built package is more useful. [Ripes](https://github.com/mortbopet/Ripes) is such a package. It comes as an appimage so just download and execute from the terminal to fire it up!

One major advantage of Ripes is that it contains simulation of external peripherals such as LED matrix and switches. Which makes it more engaging for the new-commer.

[Ripes in action](/img/ripes_animation.gif)


Stay tuned..
