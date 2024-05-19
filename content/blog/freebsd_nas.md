+++
title = "Venturing into freeBSD: Homeserver attempt 2"
date = 2024-05-19 07:27:00
draft = false

[taxonomies]
categories = ["misc"]
tags = ["NAS", "self-host", "seedbox"]

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

# Why freeBSD?

ZFS. raidz2 recommended but we will use raidz1 in our experiment! [Link](https://arstechnica.com/information-technology/2020/05/zfs-101-understanding-zfs-storage-and-performance/) for more info.

# HW setup

Here are the main parts for the build:

- [N5105 Motherboard](https://www.aliexpress.com/item/1005006221619148.html) (CPU attached)
- [Innovision 4bay NAS case](https://www.aliexpress.com/item/1005001370106988.html) comes with individual HDD bay LEDs! Spectacle when they spin up!

The N5105 is an Intel Celeron chip with a TDP of just 10W (in the RPI range..). It also has the QuickSync feature for hardware assisted Plex transcoding. :)

I got an 8GB RAM. The board accepts DDR4 Laptop SO-DIMM sticks. And also got a 120GB NVME SSD for the boot drive.

The case comes with the PSU included. SAS HDDs are cheaper, but I wasn't sure if the case support them. So went for 4x SATA 4TB ones.

# SW setup

As mentioned we'll be using freeBSD. This OS does not support docker. But has the concept of *Jails*. It is the same principle (container isolation) but predates docker.


Using a *jail manager* provides a simpler interface to managing jails. We'll use the *bastille* jail manager. For the following features:

- auto NAT! (using PF; whole new rabbithole; we'll learn just enough to get by..)
- easy and intuitive recipes/templates to automate spinning up jails (very useful in our complex case!)

We'll use no hostname resolution. Instead we'll refer each app using it's jail's hardcoded IPs (that's internally NAT'd)
        
## Pre-requisites

We add our user to sudo. In this article I'll be referring to user `ajit` as the main user. The username doesn't matter, what's important is the `uid`. I've set this to `1001` to be consistent with the default first user created in freeBSD host. See the templates below for clarity.

Remember to add this user to group `wheel` when you install the host system or afterwards. Then:

```
su -
install pkg sudo
visudo
```

Uncomment the %wheel so it looks like this :

```
## Allows people in group wheel to run all commands
%wheel    ALL=(ALL)   ALL
```

We have to enable mDNS so that our server can be reachable using it's hostname (eg freesrv.local):

```
sudo pkg install openmdns

sudo sysrc mdnsd_enable=yes
sudo sysrc mdnsd_flags=igc0 #indicate eth interface
sudo service mdnsd start

```
In the flag above replace `igc0` with your ethernet interface.

Enable `powerd` to make the system run more power efficient. See [here](https://bastian.rieck.me/blog/2013/freebsd_nas_part_ii/).

## ZFS setup

We begin by setting up our ZFS storage. Refer the [handbook](https://docs.freebsd.org/en/books/handbook/zfs/) and create `media` and `appdata` datasets. Now create the following directory structure:

```
cd <..>/media
mkdir medialibrary
mkdir medialibrary/movies
mkdir medialibrary/tv
mkdir torrents

cd <..>/appdata
mkdir radarr sonarr sabnzbd

```

After initialised, I was left with 10TB usable storage!

## Bastille jails setup

Run this to automatically setup bastille. It'll also setup PF with rule for ssh.

```
sudo bastille setup
```

We also need to allow mDNS (hostname resolution). To do this add the following to the `/etc/pf.conf`:

```
pass in inet proto udp from any to any port = 5353 keep state
```

Now:

```
sudo service pf start
```

You will need to re-initiate the ssh connection.

Finally to setup the jails issue the following commands (templates at the end of this article):

```
sudo bastille create radarrJail 14.0-RELEASE 10.17.89.50
sudo bastille template radarrJail bastillebsd-templates/radarr

sudo bastille create sonarrJail 14.0-RELEASE 10.17.89.51
sudo bastille template radarrJail bastillebsd-templates/sonarr

sudo bastille create sabnzbdJail 14.0-RELEASE 10.17.89.52
sudo bastille template radarrJail bastillebsd-templates/sabnzbd

sudo bastille create jellyfinJail 14.0-RELEASE 10.17.89.53
sudo bastille template radarrJail bastillebsd-templates/jellyfin

sudo bastille create homerJail 14.0-RELEASE 10.17.89.54
sudo bastille template homerJail bastillebsd-templates/homer
```

If everything goes to plan, you should have an accessible *ARR suite running, with the correct permisions.

TODO: Implement the equivalent of *Watchtowerr* application that auto updates the container packages on a periodic basis (ie weekly). To achieve this I could incorporate the cron and some shell scripting (but my scripting skills are not upto scratch). A rudimentary approach is outlined in this [forum post](https://forums.FreeBSD.org/threads/is-it-possible-to-pkg-update-upgrade-all-jails-in-one-go.39446/post-219246). Unfortunately it restarts the containers regardless if there was an update, which I think is wasteful.


# Templates dump

radarr:
```
CMD pw useradd -u 1001 -n ajit -c "Ajit" -s csh -m -w random
CMD mkdir /mnt/media

CONFIG set allow.mlock=1;
CONFIG set ip6=inherit;
RDR tcp 7878 7878
RESTART

PKG radarr
MOUNT /home/ajit/config/radarr /usr/local/radarr nullfs rw 0 0
MOUNT /home/ajit/media /mnt/media nullfs rw 0 0
RESTART

SYSRC radarr_enable="YES"
SERVICE radarr start
CMD chown -R ajit:ajit /var/run/radarr
CMD sed -i -r s'/radarr_user:="radarr"/radarr_user:="ajit"/' /usr/local/etc/rc.d/radarr
CMD sed -i -r s'/radarr_group:="radarr"/radarr_group:="ajit"/' /usr/local/etc/rc.d/radarr
SERVICE radarr restart

```

sonarr:
```
CMD pw useradd -u 1001 -n ajit -c "Ajit" -s csh -m -w random
CMD mkdir /mnt/media

CONFIG set allow.mlock=1;
CONFIG set ip6=inherit;
RDR tcp 8989 8989
RESTART

PKG sonarr
MOUNT /home/ajit/config/sonarr /usr/local/sonarr nullfs rw 0 0
MOUNT /home/ajit/media /mnt/media nullfs rw 0 0
RESTART

SYSRC sonarr_enable="YES"
SERVICE sonarr start
CMD chown -R ajit:ajit /var/run/sonarr
CMD sed -i -r s'/sonarr_user:="sonarr"/sonarr_user:="ajit"/' /usr/local/etc/rc.d/sonarr
CMD sed -i -r s'/sonarr_group:="sonarr"/sonarr_group:="ajit"/' /usr/local/etc/rc.d/sonarr
SERVICE sonarr restart

```

sabnzbd:
```
CMD pw useradd -u 1001 -n ajit -c "Ajit" -s csh -m -w random
CMD mkdir -p /mnt/media/torrents

CONFIG set ip6=inherit;
RDR tcp 8080 8080
RESTART

PKG sabnzbd
MOUNT /home/ajit/config/sabnzbd /usr/local/sabnzbd nullfs rw 0 0
MOUNT /home/ajit/media/torrents /mnt/media/torrents nullfs rw 0 0
RESTART

SYSRC sabnzbd_enable="YES"
SERVICE sabnzbd start
CMD chown -R ajit:ajit /var/run/sabnzbd
CMD sed -i -r s'/sabnzbd_user:="sabnzbd"/sabnzbd_user:="ajit"/' /usr/local/etc/rc.d/radarr
CMD sed -i -r s'/sabnzbd_group:="sabnzbd"/sabnzbd_group:="ajit"/' /usr/local/etc/rc.d/radarr
SERVICE sabnzbd restart

```

jellyfin:
```
CMD mkdir -p /mnt/data/movies
CMD mkdir -p /mnt/data/tv

CONFIG set allow.mlock=1;
CONFIG set ip6=inherit;
RDR tcp 8096 8096
RESTART

PKG jellyfin
MOUNT /home/ajit/media/medialibrary/movies /mnt/data/movies nullfs rw 0 0
MOUNT /home/ajit/media/medialibrary/tv /mnt/data/tv nullfs rw 0 0
RESTART

SYSRC jellyfin_enable="YES"
SERVICE jellyfin start

```

homer:
```
CONFIG set ip6=inherit;
RDR tcp 80 80
RESTART

PKG apache24

CMD curl -LO https://github.com/bastienwirtz/homer/releases/latest/download/homer.zip
CMD unzip -o homer.zip -d /usr/local/www/apache24/data
CMD cp /usr/local/www/apache24/data/assets/config.yml.dist /usr/local/www/apache24/data/assets/config.yml
CP config.yml /usr/local/www/apache24/data/assets/

SYSRC apache24_enable="YES"
SYSRC apache24_flags=""
CMD httpd -t
SERVICE apache24 start

```

homer `config.yml`. Place this file beside the homer template.
```
---
# Homepage configuration
# See https://fontawesome.com/v5/search for icons options

title: "FreeSRV dashboard"
subtitle: "Homer"
logo: "logo.png"
# icon: "fas fa-skull-crossbones" # Optional icon

header: true
footer: '<p>Created with <span class="has-text-danger">❤️</span> with <a href="https://bulma.io/">bulma</a>, <a href="https://vuejs.org/">vuejs</a> & <a href="https://fontawesome.com/">font awesome</a> // Fork me on <a href="https://github.com/bastienwirtz/homer"><i class="fab fa-github-alt"></i></a></p>' # set false if you want to hide it.

# Optional theme customization
theme: default
colors:
  light:
    highlight-primary: "#3367d6"
    highlight-secondary: "#4285f4"
    highlight-hover: "#5a95f5"
    background: "#f5f5f5"
    card-background: "#ffffff"
    text: "#363636"
    text-header: "#ffffff"
    text-title: "#303030"
    text-subtitle: "#424242"
    card-shadow: rgba(0, 0, 0, 0.1)
    link: "#3273dc"
    link-hover: "#363636"
  dark:
    highlight-primary: "#3367d6"
    highlight-secondary: "#4285f4"
    highlight-hover: "#5a95f5"
    background: "#131313"
    card-background: "#2b2b2b"
    text: "#eaeaea"
    text-header: "#ffffff"
    text-title: "#fafafa"
    text-subtitle: "#f5f5f5"
    card-shadow: rgba(0, 0, 0, 0.4)
    link: "#3273dc"
    link-hover: "#ffdd57"

# Optional navbar
# links: [] # Allows for navbar (dark mode, layout, and search) without any links
links:
  - name: "Contribute"
    icon: "fab fa-github"
    url: "https://github.com/bastienwirtz/homer"
    target: "_blank" # optional html a tag target attribute
  - name: "Wiki"
    icon: "fas fa-book"
    url: "https://www.wikipedia.org/"
  # this will link to a second homer page that will load config from additional-page.yml and keep default config values as in config.yml file
  # see url field and assets/additional-page.yml.dist used in this example:
  #- name: "another page!"
  #  icon: "fas fa-file-alt"
  #  url: "#additional-page" 

# Services
# First level array represent a group.
# Leave only a "items" key if not using group (group name, icon & tagstyle are optional, section separation will not be displayed).
services:
  - name: "Applications"
    icon: "fas fa-cloud"
    items:
      - name: "Jellyfin"
        logo: "assets/tools/sample2.png"
        subtitle: "Media Player"
        tag: "app"
        url: "http://freesrv.local:8096/"
        target: "_blank" # optional html a tag target attribute
      - name: "Radarr"
        logo: "assets/tools/sample.png"
        subtitle: "Movies"
        tag: "app"
        url: "http://freesrv.local:7878/"
        target: "_blank" # optional html a tag target attribute
      - name: "Sonarr"
        logo: "assets/tools/sample.png"
        subtitle: "TV"
        tag: "app"
        url: "http://freesrv.local:8989/"
        target: "_blank" # optional html a tag target attribute
      - name: "Sabnzbd"
        logo: "assets/tools/sample.png"
        subtitle: "Usenet"
        tag: "app"
        url: "http://freesrv.local:8080/"
        target: "_blank" # optional html a tag target attribute
      
```


