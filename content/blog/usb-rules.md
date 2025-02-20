+++
title = "Setup Atomic Desktops for embedded development"
date = 2025-02-21 06:26:00
draft = false

[taxonomies]
categories = ["misc"]
tags = ["sdk", "swd", "debug"]

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

If you're down with all the hype and use an Atomic Desktop for development, you may find this post useful in accessing the debugger/programmer from within the dev container.

I'm using Fedora Silverblue, and by default within a container the CMSIS-DAP programmer appears as:

```
crw-rw----. 1 nobody nobody 166, 0 Feb 21 05:49 /dev/ttyACM0
```

And allows no access to OpenOCD and serial consoles.

First you want to get your programmer's usb device attributes. After plugging it in, run `dmesg`:

```
[45391.238266] usb 1-4.4.1: USB disconnect, device number 12
[45397.950222] usb 1-4.4.1: new full-speed USB device number 13 using xhci_hcd
[45398.028281] usb 1-4.4.1: New USB device found, idVendor=2e8a, idProduct=000c, bcdDevice= 2.01
[45398.028297] usb 1-4.4.1: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[45398.028298] usb 1-4.4.1: Product: Debugprobe on Pico (CMSIS-DAP)
[45398.028299] usb 1-4.4.1: Manufacturer: Raspberry Pi
[45398.028300] usb 1-4.4.1: SerialNumber: DE6640A007855023
[45398.031589] cdc_acm 1-4.4.1:1.1: ttyACM0: USB ACM device
```

In my case I'm using a Pico Probe (which programs all ARM Cortex-M devices). And it indicates **idVendor=2e8a, idProduct=000c**.


Now create the following files. Making note to replace <username>.

`/etc/udev/rules.d/50-cmsis-dap.rules`:
```
# 2e8a:000c PicoProbe CMSIS-DAP
SUBSYSTEM=="usb", ATTR{idVendor}=="2e8a", ATTR{idProduct}=="000c", OWNER="<username>"
```

`/etc/udev/rules.d/50-usb-serial.rules`:
```
SUBSYSTEM=="tty", SUBSYSTEMS=="usb", OWNER="<username>"
```

Run the following, then replug the programmer:

```
sudo udevadm control --reload-rules
sudo udevadm trigger
```

Also note that this procedure is applicable to traditional distro's to use the tools without *sudo*.

