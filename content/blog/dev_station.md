+++
title = "Building a Developer's Workstation"
date = 2023-05-23 00:27:00
draft = false

[taxonomies]
categories = ["misc"]
tags = ["hardware", "workstation"]

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

In this article we will see how we build a developer's workstation that has good overall performance and suitably compact in size.

Our first decision is the OS. I may be a *little* biased here. But you may have guessed that Linux is our choice of OS. As discussed in [this article]({% post_url /update/2023-05-11-debian_testing %}), Debian is unsuitable as a daily driver. We choose it's derivative [POP! OS](https://system76.com/pop) as our OS. It is a quite popular distro that is well maintained.

Hardware wise our goto is the ex-lease HP Elitedesk 800 G2 mini desktop. Although it comes in several revisions, my recommendation is to go for the G2 version (running the 6th Gen core-i5) which I think is a good compromise between cost and performance/efficiency. I believe the G4 revision is the current one as of Q2 2023, which can be purchased new direct from HP site.

The good thing about buying an ex-lease item (other than low cost) is that you repurpose it which would have otherwise ended up straight in a landfill. And you help out the planet which is a win-win. In addition to this, Linux loves these ex-lease systems. Being a few years old and bieng in the field for a while, linux has mature drivers for it's chipsets and peripherals.

> These HP/DELL branded office desktops are very durable systems that they pose great value even as second hand ex-lease systems. That's one thing that I observed in my stint as a repair tech at a refurbished computer store.

[Here's a referral link to ebay](https://www.ebay.com/sch/i.html?_nkw=hp+elitedesk+800+g2+mini+desktop&_sacat=171957&mkcid=1&mkrid=711-53200-19255-0&siteid=0&campid=5338988127&customid=g2mini&toolid=10001&mkevt=1) if you are interested to purchase this. This gets me a small commission at no extra cost to you!

### Installing Software


The absolute first thing you must install in a fresh install of *POP! OS* (or any distro for that matter) is [Oh My Zsh](https://ohmyz.sh/). It genuinely is your terminal on steroids. It will spruce up your terminal to a whole another level. Some of it's features include:

- Predictive Completion
- Enhanced Tab completion
- Syntax highlighting (right in the shell!)

[This article](https://dev.to/kumareth/a-beginner-s-guide-for-setting-up-autocomplete-on-ohmyzsh-hyper-with-plugins-themes-47f2) has a guide on how to install it and a couple plugins to get the above functionality.

Next comes the decision on a text editor. If you'd like to go the vim route, I highly recommend taking a look at [Doom Emacs]({% post_url /update/2022-11-16-doom %}). It's *emacs* but with vim keybindings. Or you could go with the ever popular VS Code.

Finally you need to install your development environment for the framework that you use, be it web dev (node/PHP/Python etc backend) or for embedded (Zephyr in our case). If you'd like to dive into Zephyr development, take a look of my [Zephyr Guide](https://simplycreate.online/zephyr-guide/).[Nordic dev academy](https://academy.nordicsemi.com/courses/nrf-connect-sdk-fundamentals/) also has a great intro.

Have fun hackin'.
