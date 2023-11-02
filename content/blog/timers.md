+++
title = "Timers"
date = 2022-02-24
draft = false

[taxonomies]
categories = ["bl702"]
tags = ["bl702", "sdk", "timers"]

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

## Tick Timing

Basic timing, there is nothing much to it. We just need a couple of functions for basic timing. If you are familiar with tick based timing in another RTOS, it's exactly the same.

```
tickType ts, milliseconds_spent;

ts = xTaskGetTickCount();

< do stuff >

milliseconds_spent = (xTaskGetTickCount() - ts) / portTICK_RATE_MS;

```

## Hardware Timers with Interrupts

