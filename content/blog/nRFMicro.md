+++
title = "Switching to the nRFMicro"
date = 2022-03-20 00:27:00
draft = false

[taxonomies]
categories = ["misc"]
tags = ["nRF52", "embedded hobbyist", "microcontrollers"]

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

In response to my [rants](@/blog/thoughts-esp-idf.md) on the low-power wireless situation with the ESP ecosystem, I've finally (hopefully :-) ) decided instead to base on the nRF52 platform using Zephyr RTOS. These chips come with onboard low-power wireless radio including the 802.15.4; ideal for short distance networking.

My biggest gripe was that the ecosystem didn't support hobbyiest level cheap development modules. This is not the case anymore since I found this breakout board project on [github](https://github.com/joric/nrfmicro). This board can be ordered on [PCBWay](https://www.pcbway.com/project/shareproject/nRFMicro_1_4.html) for like $30 for an assembled 5 bunch.

The board lacks out-of-the box support from Zephyr, but this could easily be solved by an upstream pull-request. Also featuring the board on the Zephyr supported list could bring in some more exposure; I had to ravage through the internet to come across this project!
