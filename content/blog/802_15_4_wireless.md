+++
title = "802.15.4 Wireless"
date = 2023-04-22 00:27:00
draft = false

[taxonomies]
categories = ["misc"]
tags = ["nRF52", "low-power wireless", "zigbee"]

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

'802.15.4' is a low power low-data-rate  wireless LAN comminications protocol ideally suited for microcontrollers. And only requires roughly 10-20mA of current to operate (on a 3.3V supply). It has been widely adopted in many applications, such as wireless sensor networks, home automation, and industrial control. It is similar to the widely popular zigbee protocol but is an open standard. Which means it is shipped with opensource RTOS's for platforms that are compatible... My main gripe is that it is yet not popular among hobyists who tend to reach for WiFi in place of this low power network even in microcontroller settings...

The Nordic seires of microcontrollers well support this protocol along with BLE. Compared to BLE this is more suited for industrial noisy environments!

The IEEE 802.15.4 standard defines the physical layer (PHY) and medium access control (MAC) layer for LR-WPANs(low-rate wireless personal area networks). The PHY layer specifies the radio transmission characteristics, including the modulation scheme, data rate, and frequency band. The MAC layer provides mechanisms for channel access, data framing, and error control.

The IEEE 802.15.4 standard supports two frequency bands: 2.4 GHz and 868/915 MHz. The 2.4 GHz band is used in most LR-WPAN applications and provides a data rate of 250 kbps. The 868/915 MHz band is used in some applications that require longer range but lower data rates.

The MAC layer of IEEE 802.15.4 uses a beacon-enabled mode and a non-beacon-enabled mode. In the beacon-enabled mode, a coordinator device periodically transmits a beacon frame that contains information about the network. In the non-beacon-enabled mode, devices can transmit data at any time, without waiting for a beacon frame.

IEEE 802.15.4 also supports two types of topologies: star and peer-to-peer. In the star topology, all devices communicate with a central coordinator device. In the peer-to-peer topology, devices can communicate directly with each other.

In future articles we will see this protocol in action with the Micro:bitV2 which is equipped with the Nordic nRF52833.

