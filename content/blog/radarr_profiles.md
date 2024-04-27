+++
title = "Setup Radarr for x265 encodes"
date = 2024-04-27 17:27:00
draft = false

[taxonomies]
categories = ["misc"]
tags = ["NAS", "self-host", "seedbox"]

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

This article is a continuation of [this post](@/blog/diy_nas.md) on setting up a DIY NAS. Here we will setup Radarr/Sonarr custom formats, such that it prioritises the efficient x265 1080p encodes.

First go to Settings->Custom Formats and setup two custom formats like so:

![RalphyXD-like](/img/RalphyXD-like.png)

![Lama-like](/img/Lama-like.png)

With the appropriate conditions as shown. The x265 & x264 are preset conditions available in the drop-down!

Next go to Settings->Profiles. Select `HD - 720p/1080p` And add the custom formats as indicated below, using the `score` value to prioritise the x265.

![720p/1080p quality profile](/img/720p_1080p-quality-profile.png)

Third and final step, setup Quality Definitions. We are interested in 720p & 1080p profiles here.

![Quality Slider](/img/quality-slider.png)


Then go ahead Auto 'Search Movie' feature on top in the movie listing page to test it. Be sure to select the `HD - 720p/1080p` Quality Profile when you add the new movie!

