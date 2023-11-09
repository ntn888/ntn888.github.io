+++
template = 'home.html'

[extra]
lang = 'en'
+++

**Unleashing Potential: Your Next-Level Embedded Electronics Consulting**

Hey there, fellow electronics aficionados! Are you ready to turbocharge your neat projects and bring your innovative ideas to life? Our specialized consulting services are here to supercharge your commercial ventures with the expertise and precision they deserve.

**Why Consult?**

In the vast ocean of embedded electronics, it’s easy to feel like you're navigating uncharted waters. That’s where we step in. Our consulting services provide strategic guidance to ensure your products not only meet the market demand but set new benchmarks for innovation and efficiency.

**Tailored Guidance**

Imagine having a personal guru who not only gets your vision but also speaks the language of capacitors, breadboards, and binary. That's what you get with our consulting services. We offer personalized advice tailored to your specific needs, whether it’s selecting the perfect sensor for your home automation system or optimizing power consumption for your latest wearable.

**Cost-Effective Solutions**

We're all about maximizing impact while keeping your wallet happy. Our mantra? Brilliant doesn’t have to mean broke. We’ll help you navigate the world of affordable components and share secrets on where to snag the best deals, ensuring your project stays economically savvy.

**Innovative Problem Solving**

Stuck in a rut with a pesky problem you just can't crack? Our consulting services are like a trusty digital multimeter, ready to troubleshoot and debug. We delve into the mechanics of your project, breaking down complex challenges into manageable bites until "Eureka!" - we find the solution.

**Latest Tech Insights**

Stay ahead of the curve with insights into the latest trends and technologies. We're not just about theory; we get our hands dirty with the latest IoT gadgets, from Nordic nRFs to Raspberry Pis, ensuring you're leveraging cutting-edge tech that’ll make your project stand out.

**Networking Opportunities**

Consulting isn't just about the nitty-gritty of electronics; it's about connections. We plug you into a network of like-minded innovators, component suppliers, and tech enthusiasts. It's a community that shares, collaborates, and supports each other.

**Educational Empowerment**

Our consultancy goes beyond advice; we aim to empower your team with knowledge. Understanding the 'why' behind each decision ensures your team is equipped to maintain and develop your embedded systems independently, enhancing your in-house capabilities.

**Custom Project Development**

Got a unique project idea? We're all ears and soldering irons! Our custom project development services help transform your wildest concepts into tangible, blinking, and buzzing realities. From initial sketches to the final product, we're with you every step of the circuit.

**Continuous Support**

Our relationship doesn't end with project completion. We offer continuous support to ensure your project thrives and evolves. Whether it's a firmware update or a new feature addition, we're here to keep your project on the cutting edge.

**Join the Revolution**

So, are you ready to join the revolution of turning dreams into reality? Our embedded electronics consulting is your ticket to not just building but innovating. It's about making smarter choices, learning endlessly, and connecting deeply with the pulse of technology.

Partner with us for consulting services that promise not just to build but to innovate and lead. Together, we can craft the next generation of embedded systems that drive your business forward.

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
