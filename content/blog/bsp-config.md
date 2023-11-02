+++
title = "Board Setup Config"
date = 2022-03-15
draft = false

[taxonomies]
categories = ["bl702"]
tags = ["bl702", "sdk", "environment"]

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

The board setup configuration is included in the path ```components/platform/soc/bl702/bl702_std/BSP_Board```. In our case it's the ```bl702_evb``` sub directory. It houses three files:

- ```clock_config.h```
- ```peripheral_config.h``` 
- ```pinmux_config.h``` 

The ```clock_config.h``` can be used as-is for our board XT-ZB1 without any changes.

The complimentary online [GUI Tool](https://dev.bouffalolab.com/media/config/index.html) is handy for new users to intuitively generate these config files according to desired custom configurations.

< TODO >

*Find out:*
- how to over-ride these files locally per project
- need to manually edit the ```pinmux_config.h``` to accomodate an enabled peripheral (eg SPI)?

