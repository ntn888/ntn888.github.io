+++
template = 'home.html'

[extra]
lang = 'en'
+++

In this blog we will be concerned with the world of embedded development. I see it that the tooling and the dev environments have come a long way in the last 10 years, and it is an exciting world! This blog will be more concerned from a hobbyiest / maker viewpoint, involving parts mostly sourced from the bargain site Aliexpress.

More specifically we will be looking at IoT development. More specifically the low power wireless devices; namely BLE, Zigbee and the open standard 802.15.4. (Why they didn't give it a human friendly name I dont know?!).

Most development walkthroughs will be done on Linux environment, specifically Debian 10/11. Linux oriented articles/guides will be scattered throughout. I'm also a Linux addict. And where possible we will take a open source/open standard approach. This tends to be relevant for longer, whereas proprietary products change or dissappear!

The primary boards/chips in our attention is the

- Micro:Bit(nRF52833) (low power wireless)
- Pi PICO (Cheap all rounder)
- BL702 (wireless; abandoned for the nRF)

The BL702 is a new offering from the chip design company Bouffalo Lab, that is extremely cheap, performant RISC-V chip. With all the bells & whistles of IoT, it’s got BLE, 802.15.4, and Zigbee. See [this](https://ntn888.github.io/update/2022/02/19/xt-zb1-bl702.html) post for further info.

The development environments we will tackle are based on open source project components integrated into the respective IDE by the vendor: such as FreeRTOS and lwIP.

FreeRTOS is a real-time kernel that enables multitasking; makes the life of a firmware dev easier. Good resources about RTOS theory include:

- Digi-key [Youtube series](https://www.youtube.com/watch?v=F321087yYy4)
- μC/OS-II User’s Manual Chapter 2: “Real-Time Systems Concepts” pp47. Accessible [here](http://www.farnell.com/datasheets/1950186.pdf) is an excellent intro into the world of RTOS. It has slow paced, thorough treatment.

PS: An abandoned write-up on the linux based RTOS - Zephyr is accessible [here](https://ntn888.github.io/zephyr-guide/). It currently covers just the basics...

PS: Based off my rants about the lack of cheap hobbyiest nRF52 boards; [here's](https://github.com/ntn888/833iot) a PCB I adopted. Features a RESET button and SWD pinout; and LIPO battery charging (for those low power applications!! Why else would you want an integrated soulution?! :smiley: ). It houses the E73-2G4M08S1E modules sold in Taoboa for $2.50. It's open licensed but with interest I could accumulate locally for posting...

![pcb screenshot](/img/wSONlPv.png)
