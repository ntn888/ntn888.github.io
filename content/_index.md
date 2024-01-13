+++
template = 'home.html'

[extra]
lang = 'en'
+++

🔧 Are you tired of grappling with pesky issues in your #embeddedelectronics project? ✋ Don't fret, our team has got your back! ✨ We are a group of experts passionate about helping fellow technicians and businesses conquer their toughest obstacles. 💪

💰 Our mission is simple: we provide cost-effective embedded electronics consultancy services with unbeatable customer support.📚 Need help implementing microcontrollers or selecting the right chips? 👀 Got you covered! Trying to get a firmware optimized, no problem at all! 🙌🏼

Our goal is to be an indispensable asset that streamlines your embedded electronics development process.🚀 Let's create remarkable innovations together!👩‍💻📲 Drop us a line now and watch your project soar high like an eagle in the tech skies! 🚀 ✨

Shoot us a mail! Or connect to me on [Fiverr](https://www.fiverr.com/ajitananthadeva).

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
