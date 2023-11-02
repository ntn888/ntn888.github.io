+++
title = "My experience with Debian Testing"
date = 2023-05-11
draft = false

[taxonomies]
categories = ["update"]
tags = ["workstation", "dev-environment", "open-source"]

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

I'm a big proponent of Linux [Or GNU/Linux to be technically correct]. I like the idea of how the entire system is just a collection of individual functioning pieces of software.

Their motto is *Do one thing and do it well*. I think this is a great mantra for successful engineering altogether. That is, break down the task into simple manageable tasks and implement and integrate them!

Although it's not as popular in desktops and personal computer markets, it unanimously powers the web. A good portion of servers run linux. This is no surprise considering the reliability and flexibility of the Linux OS. To probe further, the popularity of the humble Wordpress platform drives the need for Linux web servers, as Wordpress is run on top the old school LAMP stack (Where *L* denotes the Linux part).

Among Linux servers, Debian is one of the most popular[^1]. The mainline Debian distribution is considered to be the hallmark of stability. That's great. But it comes with a downside... It's software repository is not the most upto-date. Usually you'd be pulling in updates that are lagging around two years behind the current status. So it seems Debian is not so great for powering a daily driver personal workstation.

Enter Debian Testing. Debian has two other stable stuatus of releases: `Unstable` and `Testing`. Unstable was too unstable for me. `Testing` seemed like a good middle-ground. You can read more about `Testing` [here](https://wiki.debian.org/DebianTesting). One of the pre-requisites is that the package to make it into `Testing` must have been in Unstable for atleast two days.

One major problem with these `Unstable`/`Testing` releases is that they tend to unexpectedly break software when updating. But there are ways to fortify testing. We do this with an application called [Timeshift](https://github.com/linuxmint/timeshift). As the name implies, we can backup the hard drive contents and revert the system to a previous state following an undesired outcome.

Note that `Timeshift` requires the partition to be formatted in `btrfs` type and that too in the right naming scheme. The following Youtube video has instructions on installing `Debian Testing` this proper way with `Timeshift` enabled if you want to try it out.

{{ youtube(id="IdqkjpsyUNg") }}

In my experience it wasn't good enough. I found myself reaching too often to the Timeshift backups, from things breaking after an update. Very often I had the problem of installed applications going missing.

Hence I just use POP-OS. It comes from the Debian lineage, so we have familarity there especially with the package manager `apt`. It's a very popular desktop based distribution. However I'm not found of it's `Cosmic` Desktop. I find it a butchered up version of GNOME desktop. To install the GNOME desktop I just have to run `sudo apt install gnome-session`. I run trusty Debian for servers and VPSs (like we did in the other post about Servarr as a Docker host!).

[^1]: [https://w3techs.com/technologies/details/os-linux](https://w3techs.com/technologies/details/os-linux)
