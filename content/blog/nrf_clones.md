+++
title = "nRF52840 board (Nice!Nano clones)"
date = 2023-11-07 20:27:00
draft = false

[taxonomies]
categories = ["nrf"]
tags = ["nRF52", "embedded hobbiest"]

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

These Chinese Nicenano V2 clone have started popping up on AliExpress. This is great news for us low-power-wireless nerds; As this widens our choice for ready made boards..

>Nordic hasn't actually been welcoming towards hobbyists and this has resulted in scarcity of ready-made newbie friendly boards for prototyping (as opposed to esp32 or the ST's bluepill). Which has been a real game stopper for home IOT projects. We've discussed two possible boards in [this post](@/blog/mcu-selection-guide.md). Here is a new viable option.

[NRF52840 Development Board Supermini Compatible With Nice!Nano V2.0](https://aliexpress.com/item/1005006035267231.html)

![Main board pics](nrf_clone_pic.resized.png)

![Board pinouts](nrf_clone_pinout.resized.png)

Features:

- Mini-sized (breadboard friendly)

- Built-in LiPo battery charger

- Supports charger boost jumper (100mA to 300mA) on the back side

There is a known issue with higher leak current than the original micro board. This one is reported to have a deep sleep current consumption of 700mA! ([reddit](https://redd.it/16q5b2c))

For more details and the workaround of the issue please follow the link to [this site](https://github.com/joric/nrfmicro/wiki/Alternatives#supermini-nrf52840).


