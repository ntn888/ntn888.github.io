+++
title = "Deploying an IPv6 based seedbox"
date = 2023-09-16
draft = false

[taxonomies]
categories = ["misc"]
tags = ["VPS", "IPv6", "server"]

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

With VPS that run purely IPv6 stack being cheaper (for example [HostBRR offerings](https://my.hostbrr.com/order/main/index/storage)); I'm doing a write-up on how to setup a seedbox running IPv6.

> Please keep in mind, you need to have IPv6 connectivity on your home computer/internet to access your services on the VPS. To check if you do head over to [https://ip6.biz/](https://ip6.biz/).


Install docker and docker compose on your machine first. Once this is done we can then setup our services.

Due to some of the docker image registry sources operating in ipv4 environment, they will be unreachable from our ip6 only box! We have to manually provision a method to make the interconnection work. This is only temporary though, only till we pull our images. After that we will revert to ip6 only operation.

## Change Nameservers

To achieve this translation, we will need to adjust our nameservers as follows (courtesy [https://nat64.net/](https://nat64.net/)).

```
nameserver 2a01:4f9:c010:3f02::1
nameserver 2a00:1098:2c::1
nameserver 2a00:1098:2b::1
```

Although this is not as straight forward as it seems. The file `/etc/resolv.conf` cannot be edited by hand as it will be update to any network changes. Attempting to use the package `resolvconf` deemed fruitless for me. So I resorted to the *trick* method of editing this file:

```
sudo apt install e2fsprogs -y # install needed package
cd ~
cp /etc/resolv.conf . # Make a backup of the original file
sudo rm -f /etc/resolv.conf
sudo vim /etc/resolv.conf # insert the above file contents as nameservers
sudo chattr +i /etc/resolv.conf
sudo systemctl restart networking
```

Now setup your services as needed. Docker can access and pull from any generic registries.

Once the docker pulls are done:

```
sudo chattr -i /etc/resolv.conf
sudo cp ~/resolv.conf /etc/resolv.conf
sudo systemctl restart networking
```

Now our box is back to IPv6 only state.


> One downside with this system is that you can't implement auto update of docker images using watchtower. Since once again some of the docker registeries are inaccessible.

## Enable IPv6 in Docker

One last concern is that you need to enable IPv6 networking in docker so that the containers can reach the outside world. This is explained in [Enable IPv6 support](https://docs.docker.com/config/daemon/ipv6/). Basically you have to edit the file `/etc/docker/daemon.json`.

Once that's done you need to create a new IPv6 network and use that with your containers. Here's a sample compose file to run the qBittorrent image:

```
version: "3"
services:
  qbittorrent:
    image: ghcr.io/hotio/qbittorrent
    container_name: qbittorrent
    networks:
      - ip6net		# <---- add the container to the network
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - WEBUI_PORT=8080
    volumes:
      - /home/ajit/.config/appdata/qbtorrent:/config
      - /home/ajit/downloads:/downloads
    ports:
      - 8080:8080
      - 6881:6881
      - 6881:6881/udp
    restart: unless-stopped

networks:
  ip6net:
    enable_ipv6: true
    ipam:
      config:
        - subnet: 2001:0DB8::/112

```

