+++
title = "Set-up a cross-compile environment for buildroot Linux system running on Orange PI Zero 3"
date = 2023-12-19 00:27:00
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

Assuming host system Ubuntu 22.04. Install the pre-requisite packages needed for this lab:

```
sudo apt install build-essential git autoconf bison flex texinfo help2man gawk libtool-bin libncurses5-dev unzip
```

### Setting up the TFTP server

Let’s install a TFTP server on your development workstation:

```
sudo apt install tftpd-hpa
```

Then edit the file `/etc/default/tftpd-hpa` to change this setting:

```
TFTP_DIRECTORY="/srv/tftp"
```

Now restart the service:

```
systemctl restart tftpd-hpa
```

## Grab and compile buildroot

```
git clone https://git.buildroot.net/buildroot
cd buildroot
```

Load the default config for Orange PI Zero 3. And build it. Note that we shouldn't change the kernel version as it's been assigned to a custom version compatible with Orange PI.

```
make orangepi_zero3_defconfig
make
```

Once the build process is finished you will have an image called "sdcard.img" in the output/images/ directory. Flash this image to your SD card and boot.

On login prompt enter `root` and no password. TA-DA - and you're in your very own custom linux system!

For more details see *Using a build system, example with Buildroot* chapter in the slides [bootlin embedded linux qemu slides](https://bootlin.com/doc/training/embedded-linux-qemu/embedded-linux-qemu-labs.pdf).

## Compiling a kernel module

Now we'll setup the cross-compile variables.

Include the following in your PATH:
```
export PATH="$PATH:~/buildroot/output/host/bin"
```
And set the following variables:
```
export ARCH=arm64
export CROSS_COMPILE=aarch64-linux-
```

Make a new directory somewhere in your home folder called `hello`. Inside it create a C file `hello.c`:

```C
#include <linux/module.h>    /* Needed by all modules */
#include <linux/kernel.h>    /* Needed for KERN_INFO */

int init_module(void){
    printk(KERN_INFO "Hello World!\n");

    return 0;
}

void cleanup_module(void){
    printk(KERN_INFO "Goodbye World.\n");
}

MODULE_LICENSE("GPL");

```

Create `Makefile`:

```
CC = $(CROSS_COMPILE)gcc

obj-m := hello.o
KDIR := ~/buildroot/output/build/linux-custom

all:
        $(MAKE) -C $(KDIR) M=$(PWD)

```

Noting to adjust the `KDIR` according to where you have buildroot. Assuming Homefolder here.

Now run `make`. You'll get a bunch of files as result. We are interested in `hello.ko` file.


## Serial console setup

The board has 3 pins at the top exposing the serial UART. This will act as the serial console to interact with it over the terminal. The following image from the OrangePI website indicates these pins (TX/RX).

![OPI Zero3 Pinouts](/img/opi-zero3.png)

You may link this to a PC using a ftdi chip (available extensively on AliExpress), or like what I do use a Pico-probe which is equipped with a serial-to-usb port! On the host you can use Screen or Minicom to connect.

### Transfer the compiled module

We need to transfer this file to our board. We'll use the tftp service we setup earlier.

First copy the `hello.ko` file to `/srv/tftp` directory. Then in the board's prompt:

```
cd /root
tftp -gr hello.ko <server-ip>
```

Once successfully copied, clear the system log, and load the module:

```
sudo dmesg -c
insmod hello.ko
```

Now check for the message, running `dmesg` again. To remove this module, simply `rmmod hello.ko`. Check the output again and you'll see Goodbye world.

This tests that we have a successful cross-compiler environment working to compile and study device driver development on Orange PI Zero 3 running Linux!
