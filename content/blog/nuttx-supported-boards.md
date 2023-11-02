+++
title = "NuttX Supported Boards"
date = 2022-02-18
draft = false

[taxonomies]
categories = ["nuttx"]
tags = ["nuttx", "rtos"]

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

NuttX supports a variety of boards. One of the reasons I've settled on Nuttx is it's front row support for cheap chinese boards. Namely the bluepill, esp32 and our favourite the RISC-V DT-BL10 from Doiting.

The official site page for [supported boards](https://nuttx.apache.org/docs/latest/platforms/index.html) is a stub. It infact fooled me at first...

You can simply traverse the ```boards``` folder to get an uptodate view of the supported boards. For completeness sake I'll list here the list of supported boards at the time of writing:

```
$ tree boards -dL 3
boards
├── arm
│   ├── a1x
│   │   └── pcduino-a10
│   ├── am335x
│   │   └── beaglebone-black
│   ├── c5471
│   │   └── c5471evm
│   ├── cxd56xx
│   │   ├── common
│   │   ├── drivers
│   │   └── spresense
│   ├── dm320
│   │   └── ntosd-dm320
│   ├── efm32
│   │   ├── efm32-g8xx-stk
│   │   ├── efm32gg-stk3700
│   │   └── olimex-efm32g880f128-stk
│   ├── eoss3
│   │   └── quickfeather
│   ├── imx6
│   │   └── sabre-6quad
│   ├── imxrt
│   │   ├── imxrt1020-evk
│   │   ├── imxrt1050-evk
│   │   ├── imxrt1060-evk
│   │   ├── imxrt1064-evk
│   │   └── teensy-4.x
│   ├── kinetis
│   │   ├── freedom-k28f
│   │   ├── freedom-k64f
│   │   ├── freedom-k66f
│   │   ├── kwikstik-k40
│   │   ├── teensy-3.x
│   │   ├── twr-k60n512
│   │   └── twr-k64f120m
│   ├── kl
│   │   ├── freedom-kl25z
│   │   ├── freedom-kl26z
│   │   └── teensy-lc
│   ├── lc823450
│   │   └── lc823450-xgevk
│   ├── lpc17xx_40xx
│   │   ├── lincoln60
│   │   ├── lpc4088-devkit
│   │   ├── lpc4088-quickstart
│   │   ├── lpcxpresso-lpc1768
│   │   ├── lx_cpu
│   │   ├── mbed
│   │   ├── mcb1700
│   │   ├── olimex-lpc1766stk
│   │   ├── open1788
│   │   ├── pnev5180b
│   │   ├── u-blox-c027
│   │   └── zkit-arm-1769
│   ├── lpc214x
│   │   ├── mcu123-lpc214x
│   │   └── zp214xpa
│   ├── lpc2378
│   │   └── olimex-lpc2378
│   ├── lpc31xx
│   │   ├── ea3131
│   │   ├── ea3152
│   │   └── olimex-lpc-h3131
│   ├── lpc43xx
│   │   ├── bambino-200e
│   │   ├── lpc4330-xplorer
│   │   ├── lpc4337-ws
│   │   ├── lpc4357-evb
│   │   └── lpc4370-link2
│   ├── lpc54xx
│   │   └── lpcxpresso-lpc54628
│   ├── max326xx
│   │   └── max32660-evsys
│   ├── moxart
│   │   └── moxa
│   ├── nrf52
│   │   ├── nrf52832-dk
│   │   ├── nrf52832-mdk
│   │   ├── nrf52832-sparkfun
│   │   ├── nrf52840-dk
│   │   ├── nrf52840-dongle
│   │   └── nrf52-feather
│   ├── nuc1xx
│   │   └── nutiny-nuc120
│   ├── phy62xx
│   │   └── phy6222
│   ├── rp2040
│   │   ├── common
│   │   ├── pimoroni-tiny2040
│   │   └── raspberrypi-pico
│   ├── s32k1xx
│   │   ├── s32k118evb
│   │   ├── s32k144evb
│   │   ├── s32k146evb
│   │   ├── s32k148evb
│   │   └── ucans32k146
│   ├── sam34
│   │   ├── arduino-due
│   │   ├── flipnclick-sam3x
│   │   ├── sam3u-ek
│   │   ├── sam4cmp-db
│   │   ├── sam4e-ek
│   │   ├── sam4l-xplained
│   │   ├── sam4s-xplained
│   │   └── sam4s-xplained-pro
│   ├── sama5
│   │   ├── giant-board
│   │   ├── sama5d2-xult
│   │   ├── sama5d3x-ek
│   │   ├── sama5d3-xplained
│   │   └── sama5d4-ek
│   ├── samd2l2
│   │   ├── arduino-m0
│   │   ├── circuit-express
│   │   ├── samd20-xplained
│   │   ├── samd21-xplained
│   │   └── saml21-xplained
│   ├── samd5e5
│   │   ├── metro-m4
│   │   └── same54-xplained-pro
│   ├── samv7
│   │   ├── common
│   │   ├── same70-qmtech
│   │   ├── same70-xplained
│   │   └── samv71-xult
│   ├── stm32
│   │   ├── axoloti
│   │   ├── b-g431b-esc1
│   │   ├── b-g474e-dpow1
│   │   ├── clicker2-stm32
│   │   ├── cloudctrl
│   │   ├── common
│   │   ├── emw3162
│   │   ├── et-stm32-stamp
│   │   ├── fire-stm32v2
│   │   ├── hymini-stm32v
│   │   ├── maple
│   │   ├── mikroe-stm32f4
│   │   ├── nucleo-f103rb
│   │   ├── nucleo-f207zg
│   │   ├── nucleo-f302r8
│   │   ├── nucleo-f303re
│   │   ├── nucleo-f303ze
│   │   ├── nucleo-f334r8
│   │   ├── nucleo-f410rb
│   │   ├── nucleo-f412zg
│   │   ├── nucleo-f429zi
│   │   ├── nucleo-f446re
│   │   ├── nucleo-f4x1re
│   │   ├── nucleo-g431kb
│   │   ├── nucleo-g431rb
│   │   ├── nucleo-l152re
│   │   ├── olimexino-stm32
│   │   ├── olimex-stm32-e407
│   │   ├── olimex-stm32-h405
│   │   ├── olimex-stm32-h407
│   │   ├── olimex-stm32-p107
│   │   ├── olimex-stm32-p207
│   │   ├── olimex-stm32-p407
│   │   ├── omnibusf4
│   │   ├── photon
│   │   ├── shenzhou
│   │   ├── stm3210e-eval
│   │   ├── stm3220g-eval
│   │   ├── stm3240g-eval
│   │   ├── stm32butterfly2
│   │   ├── stm32f103-minimum
│   │   ├── stm32f334-disco
│   │   ├── stm32f3discovery
│   │   ├── stm32f411e-disco
│   │   ├── stm32f411-minimum
│   │   ├── stm32f429i-disco
│   │   ├── stm32f4discovery
│   │   ├── stm32ldiscovery
│   │   ├── stm32_tiny
│   │   ├── stm32vldiscovery
│   │   └── viewtool-stm32f107
│   ├── stm32f0l0g0
│   │   ├── b-l072z-lrwan1
│   │   ├── nucleo-f072rb
│   │   ├── nucleo-f091rc
│   │   ├── nucleo-g070rb
│   │   ├── nucleo-g071rb
│   │   ├── nucleo-l073rz
│   │   ├── stm32f051-discovery
│   │   └── stm32f072-discovery
│   ├── stm32f7
│   │   ├── nucleo-144
│   │   ├── stm32f746g-disco
│   │   ├── stm32f746-ws
│   │   └── stm32f769i-disco
│   ├── stm32h7
│   │   ├── nucleo-h743zi
│   │   ├── nucleo-h743zi2
│   │   └── stm32h747i-disco
│   ├── stm32l4
│   │   ├── b-l475e-iot01a
│   │   ├── nucleo-l432kc
│   │   ├── nucleo-l452re
│   │   ├── nucleo-l476rg
│   │   ├── nucleo-l496zg
│   │   ├── stm32l476-mdk
│   │   ├── stm32l476vg-disco
│   │   └── stm32l4r9ai-disco
│   ├── stm32l5
│   │   ├── drivers
│   │   ├── nucleo-l552ze
│   │   └── stm32l562e-dk
│   ├── stm32u5
│   │   ├── b-u585i-iot02a
│   │   └── drivers
│   ├── str71x
│   │   └── olimex-strp711
│   ├── tiva
│   │   ├── dk-tm4c129x
│   │   ├── eagle100
│   │   ├── ekk-lm3s9b96
│   │   ├── launchxl-cc1310
│   │   ├── launchxl-cc1312r1
│   │   ├── lm3s6432-s2e
│   │   ├── lm3s6965-ek
│   │   ├── lm3s8962-ek
│   │   ├── lm4f120-launchpad
│   │   ├── tm4c123g-launchpad
│   │   └── tm4c1294-launchpad
│   ├── tms570
│   │   ├── launchxl-tms57004
│   │   └── tms570ls31x-usb-kit
│   └── xmc4
│       ├── xmc4500-relax
│       └── xmc4700-relax
├── avr
│   ├── at32uc3
│   │   └── avr32dev1
│   ├── at90usb
│   │   ├── micropendous3
│   │   └── teensy-2.0
│   └── atmega
│       ├── amber
│       ├── arduino-mega2560
│       └── moteino-mega
├── dummy
├── hc
│   └── m9s12
│       ├── demo9s12ne64
│       └── ne64badge
├── mips
│   ├── pic32mx
│   │   ├── mirtoo
│   │   ├── pic32mx7mmb
│   │   ├── pic32mx-starterkit
│   │   ├── sure-pic32mx
│   │   └── ubw32
│   └── pic32mz
│       ├── chipkit-wifire
│       ├── flipnclick-pic32mz
│       └── pic32mz-starterkit
├── misoc
│   └── lm32
│       └── misoc
├── or1k
│   └── mor1kx
│       └── or1k
├── renesas
│   ├── m16c
│   │   └── skp16c26
│   ├── rx65n
│   │   ├── rx65n
│   │   ├── rx65n-grrose
│   │   ├── rx65n-rsk1mb
│   │   └── rx65n-rsk2mb
│   └── sh1
│       └── us7032evb1
├── risc-v
│   ├── bl602
│   │   └── bl602evb
│   ├── c906
│   │   └── smartl-c906
│   ├── esp32c3
│   │   └── esp32c3-devkit
│   ├── fe310
│   │   └── hifive1-revb
│   ├── k210
│   │   └── maix-bit
│   ├── litex
│   │   └── arty_a7
│   ├── mpfs
│   │   ├── common
│   │   ├── icicle
│   │   └── m100pfsevp
│   ├── qemu-rv
│   │   └── rv-virt
│   └── rv32m1
│       └── rv32m1-vega
├── sim
│   └── sim
│       └── sim
├── sparc
│   ├── bm3803
│   │   └── xx3803
│   └── bm3823
│       └── xx3823
├── x86
│   └── qemu
│       └── qemu-i486
├── x86_64
│   └── intel64
│       └── qemu-intel64
├── xtensa
│   ├── esp32
│   │   ├── common
│   │   ├── esp32-devkitc
│   │   ├── esp32-ethernet-kit
│   │   ├── esp32-wrover-kit
│   │   └── ttgo_lora_esp32
│   ├── esp32s2
│   │   ├── common
│   │   └── esp32s2-saola-1
│   └── esp32s3
│       ├── common
│       └── esp32s3-devkit
├── z16
│   └── z16f
│       └── z16f2800100zcog
└── z80
    ├── ez80
    │   ├── ez80f910200kitg
    │   ├── ez80f910200zco
    │   ├── makerlisp
    │   └── z20x
    ├── z180
    │   └── p112
    ├── z8
    │   ├── z8encore000zco
    │   └── z8f64200100kit
    └── z80
        └── z80sim

337 directories
```
