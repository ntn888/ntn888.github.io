+++
title = "Booting a custom device tree file source in buildroot"
date = 2023-12-21 15:27:00
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

In the [previous article](@/blog/buildroot-setup.md) we saw how to setup the default system image for Orange PI Zero 3 using buildroot. We got the basic system up and running. Now, when we do Device Driver Development, we will have to modify the device tree source.

The default dt source for our board is in the kernel sources in:

```
arch/arm64/boot/dts/allwinner/sun50i-h616-orangepi-zero3.dts
```

In this exercise we will modify the model name to something different, `OrangePi Zero3-custom`. To do this we will create a new dts file in the buildroot directory, include in it the original dts file (noted above), and override the `model` property.

Now create the following file `~/buildroot/sun50i-h616-orangepi-zero3-custom.dts`:

```
// SPDX-License-Identifier: GPL-2.0
#include "/home/ajit/opi/buildroot/output/build/linux-custom/arch/arm64/boot/dts/allwinner/sun50i-h616-orangepi-zero3.dts"
/ {
        model = "OrangePi Zero3-custom";
        compatible = "xunlong,orangepi-zero3", "allwinner,sun50i-h616";

};

```

In menuconfig set the value of `BR2_LINUX_KERNEL_CUSTOM_DTS_PATH` to `~/buildroot/sun50i-h616-orangepi-zero3-custom.dts` our custom dts file location. And unset the `BR2_LINUX_KERNEL_INTREE_DTS_NAME`.

Once that's done you're ready to do the build. Here we will be doing an incremental build (since we have already built the system earlier).

```
make linux-rebuild
```

This will (in our case) only compile the dtb file and re-install some modules. To do the sdcard image generation run:

```
make
```

>There is a caveat I noticed with uboot. You now need to set `CONFIG_DEFAULT_FDT_FILE` in uboot menuconfig to use this custom dtb. But unfortunately our board include files do not honour this setting, and in boot time, uboot proceeds to fetch the default dtb! The work-around is to set the right dtb file on uboot prompt explained below.

Flash the sdcard and boot.

At uboot prompt provide these commands to boot Linux using our custom dtb.

```
setenv fdtfile sun50i-h616-orangepi-zero3-custom.dtb
boot
```

This is the workaround that worked for me and in the boot log you can see the custom model that we set: `OrangePi Zero3-custom`. You may also cat the following file:

```
cat /sys/firmware/devicetree/base/model
```


If you get any further issues you can also try to manually boot as follows:

```
setenv bootargs "root=/dev/mmcblk1p1 rootfstype=ext4"

ext4load mmc 0:1 ${kernel_addr_r} boot/Image
ext4load mmc 0:1 ${fdt_addr_r} boot/sun50i-h616-orangepi-zero3-custom.dtb
booti ${kernel_addr_r} - ${fdt_addr_r}
```

