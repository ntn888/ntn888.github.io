+++
title = "Switching to the ESP32-C3"
date = 2022-03-18 00:27:00
draft = false

[taxonomies]
categories = ["misc"]
tags = ["fimware develompent", "embedded hobbiest", "microcontrollers"]

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

This blog will here-on switch to the esp32-c3 controller...

It is noted that the esp32 chips are infamous for higher power consumption, especially with the radio on. But it's expected that this problem will disappear with the introduction of the upcomming ESP32-H2 MCU.

The rationale for the switch is the incomplete and nonfunctional tooling on the Buffalo Lab boards, especially the inability to use JTAG debugging.

The esp32-c3 is a similarly low cost solution with a mature SDK and built-in debugger.
