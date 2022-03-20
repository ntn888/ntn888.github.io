---
layout: single
title:  "Thoughts on the ESP-IDF SDK"
date:   2022-03-19 00:27:43 +1100
categories: misc

---

By contrast the esp-idf SDK feels intuitive and very matured. This is a given, considering the popularity of the ESP chips. My only gripe about this ecosystem is the lack of lowpower wireless module. Although this is being fulfilled with the advent of the upcoming ESP32-H2, it's a long overdue wait!

> Even though the existing chips feature a BLE radio; it's not as power efficient as it should be, negating any possibility of deploying into low-power applications.


I did consider the well know nRF52840 chip targetting low power wireless applications. A huge advantage here is that it's already available! But most dev-boards, including simple ones like Adafruit Feather and Arduino Nano 33 BLE go for a whopping 40-50usd! Even the company's own (hideous looking) nRF52 dongle is 15usd. Which in my view makes it inaccessible for hobbyist applications.


In the end, this allows me to trial out the esp-idf SDK with the on-hand chip: esp32-c3, while waiting for the H2.

I personally think this is a shameful situation in the maker world in this day and age. Or I might be missing something.
