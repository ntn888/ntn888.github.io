+++
template = 'home.html'

[extra]
lang = 'en'
+++

**Unleashing Potential: Your Next-Level Embedded Electronics Consulting**

Hey there, fellow electronics aficionados! Are you ready to turbocharge your neat projects and bring your innovative ideas to life? I specialise in consulting services that are here to supercharge your commercial ventures with the expertise and precision they deserve.

**Why Consult?**

In the vast ocean of embedded electronics, it’s easy to feel like you're navigating uncharted waters. That’s where we step in. Our consulting services provide strategic guidance to ensure your products not only meet the market demand but set new benchmarks for innovation and efficiency.

Our embedded electronics consulting is your ticket to not just building but innovating. It's about making smarter choices, learning endlessly, and connecting deeply with the pulse of technology.

Partner with us for services that promise not just to build but to innovate and lead. Together, we can craft the next generation of embedded systems that drive your business forward.

Shoot us a mail!

---

In this blog we will be concerned with the world of embedded development. I see it that the tooling and the dev environments have come a long way in the last 10 years, and it is an exciting world! This blog will be more concerned from a hobbyiest / maker viewpoint, involving parts mostly sourced from the bargain site Aliexpress.

More specifically we will be looking at IoT development. More specifically the low power wireless devices; namely BLE, Zigbee and the open standard 802.15.4. (Why they didn't give it a human friendly name I dont know?!).

Most development walkthroughs will be done on Linux environment, specifically Debian 10/11. Linux oriented articles/guides will be scattered throughout. I'm also a Linux addict. And where possible we will take a open source/open standard approach. This tends to be relevant for longer, whereas proprietary products change or dissappear!

The development environments we will tackle are based on open source project components integrated into the respective IDE by the vendor: such as FreeRTOS and lwIP.

Even though this is a hobbyist blog, this one is a little different. We would not be using Arduino. Instead we would be using the above frameworks. This might be a controvesial choice, but I feel that closer-to-metal frameworks are more exciting.. This blog is an aide to move out from Arduino to more resource friendly frameworks such as FreeRTOS or RIOT-OS.

FreeRTOS is a real-time kernel that enables multitasking; makes the life of a firmware dev easier. Good resources about RTOS theory include:

- Digi-key [Youtube series](https://www.youtube.com/watch?v=F321087yYy4)
- μC/OS-II User’s Manual Chapter 2: “Real-Time Systems Concepts” pp47. Accessible [here](http://www.farnell.com/datasheets/1950186.pdf) is an excellent intro into the world of RTOS. It has slow paced, thorough treatment.

PS: An abandoned write-up on the linux based RTOS - Zephyr is accessible [here](https://ntn888.github.io/zephyr-guide/). It currently covers just the basics...

PS: Based off my rants about the lack of cheap hobbyiest nRF52 boards; [here's](https://github.com/ntn888/833iot) a PCB I adopted. Features a RESET button and SWD pinout; and LIPO battery charging (for those low power applications!! Why else would you want an integrated soulution?! :smiley: ). It houses the E73-2G4M08S1E modules sold in Taoboa for $2.50. It's open licensed but with interest I could accumulate locally for posting...

![pcb screenshot](/img/wSONlPv.png)
