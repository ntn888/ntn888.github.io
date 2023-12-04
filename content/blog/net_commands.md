+++
title = "Basic commands for Network Troubleshooting"
date = 2023-05-16 00:27:00
draft = false

[taxonomies]
categories = ["misc"]
tags = ["networking", "troubleshooting"]

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

In this post we will look at a couple commands most used for network troubleshooting.

## ping

`ping` is the most simplest network command that everyone is familiar with. You can use it to check if a node (or device) is connected and running. Note however, that if this feature has been disabled on the node it will not respond to pings!

The command takes the following format:

```
ping <target_ip>
```

## traceroute

`traceroute` is a more fancy command. It gives us the `ping` results of each hop (eg routers) along the packet's path. So it can be used to deduce where in the network the packet is failing. The command takes the following format:

```
traceroute <target_ip>
```

For example it can be used to troubleshoot problems with our selfhosted email [scenario]({% post_url /update/2022-12-27-postfix_mail %}). By running the command aimed at the Google's mail servers (for example) from our box, we can deduce if the email is failing whithin the VPS provider's network. Which would mean the provider has blocked `port 25`. In this case we would get the following behaviour.

```
# traceroute -n -T -p 25 gmail-smtp-in.l.google.com
traceroute to gmail-smtp-in.l.google.com (173.194.204.27), 30 hops max, 60 byte packets
1 192.210.145.2 0.033 ms 0.012 ms 0.011 ms
2 * * *
3 * * *
4 * * *
5 * * *
.
.
.
30 * * *
```

Since there is no reponse from the very next node it is likely failing within the provider's network and we can say that the provider is actively blocking it.

## nc

This one is called netcat. And it can be used to pass messages between the computers through network sockets. If you work on building an IoT device you will find this command incredibly useful.

You can run it in server and client mode. To run it in server mode:

```
nc -l <port>
```

To runt it in client mode:

```
echo "hello there" | nc <remote-ip> <remote-port>
```

You get the idea. If you omit the pipe command and run only the `nc` command, you'll get an interactvie prompt. Much like a telnet session!

As you can see this is a great way to set up makeshift listening servers on a specific port for IoT devices ad clients to connect to and debug the communication.
