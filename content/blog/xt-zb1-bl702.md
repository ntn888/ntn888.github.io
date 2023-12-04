+++
title = "THE Dev board you've been waiting for: $1.8 XT-ZB1 Zigbee & BLE devkit features BL702 RISC-V module"
date = 2022-02-20 00:27:00
draft = false

[taxonomies]
categories = ["update"]
tags = ["risc-v", "bl702", "zigbee"]

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

I chanced upon the XT-ZB1 on ali-express on the lookout for the perfect IoT board... Coming at around 1.8usd this is more than perfect, this is a surprise! Made by the same company that makes the SOC for the DT-BL10, Bouffalo Labs, I think this is a new frontier in IoT development. I think we probably should thank the RISC-V movement for making such things possible.

![XT-ZB1 pinouts](/img/XT-ZB1.jpg)

The MCU specs at: BL702C 32-bit RISC-V microcontroller @ 144 MHz with FPU, 132KB RAM, 192KB ROM with 8MB embedded flash. That's decent enough for most of demanding applications.

There's one caveat however, the zigbee supporting SDK variant is buried in the indicated site: www.bl602.fun, one has to download the tar copy of the SDK there.

Debugging is achieved using the advertised Sipeed RV-Debugger also available on Aliexpress. Alternatively one may use the OpenOCD's ft232r technique discussed here in an earlier post.
