+++
title = "A DIY jtag debugger"
date = 2022-02-16
draft = false

[taxonomies]
categories = ["misc"]
tags = ["diy", "swd", "debug"]

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

One of openocd's hidden perks is it could use a typical ftdi232rl USB to Serial converter into a makeshift JTAG programmer. Not only is this cheap, it's quick convenience than to wait and order a new programmer.

In this article we see how to put this to use...

<!-- more -->

First we have to install openocd if you haven't already. This could be done using your systems package manager. Or if you're installing manually be sure to enable the interface when building openocd in the following step:
```
./configure --enable-ft232r
```

Hookup your board to the ftdi board. Follow the following pinouts:

| JTAG Pin | ftdi Pin     |
| ----------- | ----------- |
| TDI      | RXD          |
| TCK      | TXD          |
| TDO      | RTS          |
| TMS      | CTS          |
| TRST     | DTR          |
| SRST     | DCD          |

Here's a pic with the debugger connected to a DT-BL10 (RISC-V) board:

![ftdi connection](/img/debugger.jpg)

Then you need to fireup openocd with a config. To do this, create a file called ```my_ft232r.cfg``` somewhere in you home directory with the following contents:
```
adapter driver ft232r
adapter speed 3000
ft232r_restore_serial 0x15
```
Now fireup openocd:
```
openocd -f path/to/my_ft232r.cfg/
```
Hopefully your target is detected!
