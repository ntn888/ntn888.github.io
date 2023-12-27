+++
title = "Milk-V Duo: $5 Embedded Linux board"
date = 2023-12-27 15:27:00
draft = false

[taxonomies]
categories = ["embedded-linux"]
tags = ["embedded-linux", "linux", "buildroot"]

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

In my journey of learning Linux Device Drivers, I have since migrated to a new target. I've switched to the [Milk-V Duo](https://milkv.io/duo) board. I find that this board has a bigger 'embedded' focused community than the OrangePI Zero3. The [forums](https://community.milkv.io/) are fairly active and people occasionally showcase their driver development projects; which I think are a great learning resource too!

Additionally the board is far cheaper, costing just 5usd, and is breadboard friendly.

---

One caveat with buildroot (we need to use a custom [repository](https://github.com/milkv-duo/milkv-duo-buildroot)) and this board is that the device tree dtb is adopted by the kernel from the uboot sources! ie. There is no separate dts file in the kernel sources. So you must edit the file `milkv-duo-buildroot/output/build/uboot-v2021.10_64mb/arch/riscv/dts/cv1800b_milkv_duo_sd.dts` (after an initial build) then run `make uboot-rebuild` and `make` to include custom device tree modifications. Don't forget to keep a backup of this file somewhere outside buildroot directory as you will lose this file upon `make clean`.


