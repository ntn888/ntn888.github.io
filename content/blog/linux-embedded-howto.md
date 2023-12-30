+++
title = "Learning Embedded Linux Driver Development"
date = 2023-12-31 03:27:00
draft = false

[taxonomies]
categories = ["embedded-linux"]
tags = ["embedded-linux", "linux", "linux drivers", "buildroot"]

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


There are two kinds of developers that become embedded/driver developers. One set is from a CS / Application developer background that drop down to low-level development. The rest is EE background that turn into developers. Of course both courses have their pros and cons. But I'd argue comming from a Hardware background has it's perks..

Regardless, here's a guide to jumping into Linux driver development from the HW background perspective. Its a collection of resources to guide your learning.

The first step is to tackle general Linux user administration.

- [The Linux Command Line](https://linuxcommand.org/tlcl.php) is freely available to download. I think getting familiar with the command line is the first major hurdle in learning Linux. This book helps with that and some BASH scripting too. (I've been putting off learning scripting for the longest time!)

- [How Linux Works](https://www.amazon.com/How-Linux-Works-Brian-Ward/dp/1718500408) is an amazing succinct guide in general Linux admin/usage.

- [Mastering Embedded Linux Programming](https://www.amazon.com/Mastering-Embedded-Linux-Programming-potential/dp/1789530385) In contrast to what the title implies (misleading) this book deals with setting up a linux system for an embedded target and discusses the workflow for cross-compilation. And a great one at that! Though this book discusses Yocto I recommend Buildroot for beginners.

- [Linux From Scratch (LFS)](https://www.linuxfromscratch.org/) is a free guide on how to compile and build a working linux system step-by-step from scratch! Grab that old laptop/machine and follow along! This step is optional but entirely worth it!

Once you have this under your belt you can start development activities.

- [The Linux Programming Interface](https://www.amazon.com/Linux-Programming-Interface-System-Handbook/dp/1593272200) Learn POSIX application API here. It's a large book; skim on what's needed. I recommend learning atleast *pthreads*.

- [Linux Device Driver Development](https://www.amazon.com/Linux-Device-Driver-Development-development/dp/1803240067) The crux of our study. Pair it up with [The Linux Kernel Module Programming Guide](https://sysprog21.github.io/lkmpg/).

At this stage you need a good relevant development board to follow along. I recommend the [Milk-V Duo](@/blog/linux-milkv-duo.md) for its breadboard friendly. It supports Buildroot and the documentation is highly embedded Linux focussed.

