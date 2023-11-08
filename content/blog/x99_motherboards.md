+++
title = "Building a Frugal Gaming PC"
date = 2023-11-08 11:27:00
draft = false

[taxonomies]
categories = ["misc"]
tags = ["hardware", "workstation", "gaming PC"]

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

**Motherboards: The Refurbished x99 AliExpress Special**

Here we will talk about a new obsession of mine - refurbished LGA 2011v3 socket motherboards; paired up with used Intel E5 1600/2700 series processor forms the basis for a cheap gaming system. These are sold in AliExpress as a [motherboard, CPU, RAM] combo for upwards of USD80 depending on the manufacturer.

There's good information about these boards on the Russian site: [https://xeon-e5450.ru/](https://xeon-e5450.ru/). Just use *Translate Web Pages* plugin for your browser to auto-translate.

There's also the Miyconst [Youtube Channel](https://github.com/miyconst/Mi899/blob/master/src/Mi899/README.md) that provides good information.

When I purchased, I personally went for the QIYIDA AliExpress seller (the product had tons of good reviews). And received the [x99h v1.41 board](https://xeon-e5450.ru/socket-2011-3/x99h/). According to Miyconst this is a poor board. Mainly due to having apparently only single DDR RAM channel.

I also noticed that the motherboard doesn't support *Suspend* functionality. In the **AHCI** options in BIOS, *Suspend* value is permanently set to *Disabled*.

The one [I purchased](https://www.aliexpress.com/item/1005004519470412.html) came with the following combo:

- x99h v1.41 board (QIYIDA)
- Intel Xeon E5 2670v3 processor (used)
- 16G (2x 8G) RAM QIYIDA brand (refurbished)

It was priced at $110 on Jan 2023.

From my personal experience, I'd advice to do thorough checks (using the sources provided above) and not just rely on AliExpress rankings/ratings. I regret my purchase as I cannot put my PC to sleep (a functionality I heavily rely on).

The whole ecosystem is possible because of the enormous market of the used Xeon chips. The 2670v3 sells on Ali for a grand total of $7! But I recommend buying as a combo for a piece of mind due to any compatiblity issues.

Last comes our most important component: A Graphics card. Ali has us covered in this regard as well! For around $80 you can get a Radeon RX580 8G! Unbeatable! I believe you can go with the AliExpress reviews on this one.

On the bright side though (going with the Xeon 2670v3 processor, which has 12 cores 24 threads), it's refreshing to see the *System Monitor* report the 24 threads available on the system!

![System Monitor screenshot](/img/xeon-monitor-24-threads.png)

