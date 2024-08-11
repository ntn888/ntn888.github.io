+++
title = "Venture into Silicon Labs' EFR32BG22 Series 2 chips"
date = 2024-08-11 13:24:00
draft = false

[taxonomies]
categories = ["efr32"]
tags = ["efr32", "Silicon Labs", "low-power wireless", "microcontrollers"]

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

Silicon Labs is an industry leader in low-power wireless chips. But Silicon Labs uses a more traditional/familiar tooling and dev environment (as opposed to Nordic's Zephyr, which is full on and complex) and can be considered less intimidating to someone approaching from a hardware/firmware background.

In this series of articles we shall see setting up a dev environment for a Linux host and working with the basic and crucial peripherals.


Resources provided to get upto speed with the chip family are:

- [2 hour long webinar introducing the SDK](https://www.silabs.com/support/training/ssv5-project-config-gecko-platform)

- [examples on github](https://github.com/SiliconLabs/peripheral_examples)

- [Doxygen dump of defines](https://docs.silabs.com/gecko-platform/5.0.1/platform-overview/)


I will be using this cheap [BG22 Bluetooth SoC Explorer Kit](https://www.silabs.com/development-tools/wireless/bluetooth/bg22-explorer-kit?tab=overview) to demo the applications.

