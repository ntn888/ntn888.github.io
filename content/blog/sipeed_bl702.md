+++
title = "Sipeed BL702 board"
date = 2022-12-01 00:27:00
draft = false

[taxonomies]
categories = ["update"]
tags = ["bl702", "risc-v", "zigbee"]

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

A well known Chinese kits maker Sipeed, has [started selling](https://www.aliexpress.us/item/1005005012406688.html) a micro board based on the BL702 chip! With some additional peripherals such as an accelerometer and a pixie LED, the board retails for only 4USD. More information can be found [here](https://wiki.sipeed.com/hardware/en/maixzero/sense/maix_zero_sense.html).
It even has the 4 debug pins collated together on one side! Good ergonomics!

![Main board with debugger](/img/sipeedM0.jpg)

This release from Sipeed paves way for more exposure and popularity which brings more adoption... And in turn more polished toolset and kits. Looking back into the bl_mcu_sdk after some months, there seems to have been an overhaul on the API. Although I'm against making changes (and breaking user's codebase), hopefully this brings better convenence and features!


Considering that the ideal target application is battery powered uses, the board does come with a port for battery input. But it lacks capability for LiPO charging. But this could be easily fixed with an external module such as [this](https://www.aliexpress.com/item/1005003781466639.html).

Happy Hacking!
