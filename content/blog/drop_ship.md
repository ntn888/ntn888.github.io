+++
title = "My experience with drop-ship warehousing"
date = 2023-05-17
draft = false

[taxonomies]
categories = ["update"]
tags = ["drop-ship", "supply chain"]

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

I had an idea to pakcage, ship and store these [bl702 devkits](@/blog/xt-zb1-bl702.md) into a US warehouse ready to locally distribute them. This is the story of how I failed misarably at it. Basically I purchased a *lot* of 100 pieces of these devkits to be stored into a warehouse to be shipped locally in the US. Although this never translated into sales; it did have many hurdles.

First was in purchasing itself. Basically I couldn't purchase from the ALL FAMILIAR Aliexpress. Because I had to locally ship it into the ligistic company's site in China; I could only ship to an international address. So Aliexpress was out of the question. For Chinese Domestic purchase, the supplier had a store in Taobao. This is fully in Chinese interface. Obviously.

Trying to register on the PC via the web portal did not succeed due to geographical blocks by Taobao. I did manage to register on the mobile app however. But unlike on the browser there's no way to easily translate for example with the Google translate plugin. What I had to do instead painstakingly, point another phone with Google lens!

That's one hurdle overcome. I had to fill in the local logistical company's address and all the details holding up on the other phone.

It took two days for the goods to reach the logistic's warehouse in China. They arrived in a box packaged with the 100 units.

Now the logistics company I went with was called [Nextsmartship](https://www.nextsmartship.com/). My experience was largely positive with them. There was a another caveat here. When using this logistics company, it is way cheaper to prepackage into individual ready-to-mail packages here in the Chinese warehouse than to ship in bulk first and parcel them for courier there. It worked out to be $0.99 per piece here.

So it would be ideal to pre-package them into appropriate package unit sizes before cargo-ing to the US: for example 1pc, 5pc and 10pc units. I also think they did a very good job of safely packaging them into individual pieces.

At the time I dealt with them, they did not have automated integration with 3rd party sites (such as woo-commerce which I used). So once you make a sale on your site you had to manually message them (on Whatsapp). But this never happened in my instance.

As you see, I think one of the requirements of selling products on your own hosted site, to even gain some exposure, you'd have to be listed on Google merchant. This is the list of shopping products you'd get when a buyer clicks on the *Shopping* tab on the Google search page.

I didn't realise at the time, but you'd need a complete site with "Privacy Policy"/"Returns Policy" etc to get approved in Google Merchant. Google outright dismisses your site for *misrepresentation* for this. I mis-took it for selling my products that was aimed at the Chinese market... So I never bothered to probe further on this. And the whole thing went down the drain.

However at this point, I myself started to get frustrated with this devkit's SDK: it's lack of documentation and more importantly the everchanging updates to the SDK that breaks backwards compatibility. As I have pointed out earlier I moved on to the Nordic nRF chips. Due to lack of hobbiest sized boards (think bluepill). I put together the [833iot](https://github.com/ntn888/833iot) for a self-contained module based off nRF52833 sold in Alibaba. Due to previous failures I never attempted to sell them.

![833iot pcb](/img/wSONlPv.png)

Even though low-power wireless and `Thread` are increasingly becoming popular, the hobbiest market is still flooded with the power-hungry WiFi chips. This protocol is so inappropriate for microcontrollers!
