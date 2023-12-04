+++
title = "Analog Filters Primer"
date = 2022-02-27 00:27:00
draft = false

[taxonomies]
categories = ["misc"]
tags = ["analog", "electronics", "electronic filter"]

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

Noise and interference is inherent in any electrical/electronic system. Their effects are especially prominent in analog systems. Most if not all embedded systems are connected to these analog signal lines and fall prey to this problem. Increasingly we find ready made modules that have digitised interface to the analog sensor. But there are times when we struck upon a novel problem and have to deal it ourselves.

Among other things analog filters help mitigate this problem. It is very often the case that this falls in scope of the task of an embedded/firmware developer due to being fairly connected.

Here we see the available resources to help build know-how of analog filtering. The following is a list of copy-left books and resources in sequential progressive order:

[DC Electrical Circuit Analysis](https://open.umn.edu/opentextbooks/textbooks/dc-electrical-circuit-analysis-a-practical-approach-fiore)

[AC Electrical Circuit Analysis](https://open.umn.edu/opentextbooks/textbooks/883)

[Semiconductor Devices](https://open.umn.edu/opentextbooks/textbooks/semiconductor-devices-theory-and-application)

[Operational Amplifiers & Linear Integrated Circuits](https://open.umn.edu/opentextbooks/textbooks/operational-amplifiers-linear-integrated-circuits-theory-and-application-3e)

>You may use simulation on PC to experiment and follow along with the above texts. Kicad has a simulation mode which can be used after entering the schematic in eeschema. Look here: [http://ngspice.sourceforge.net/ngspice-eeschema.html](http://ngspice.sourceforge.net/ngspice-eeschema.html)


Also a prerequisite to above circuit analysis is some basic highschool math: calculus and complex numbers. Refer:

[Calculus: Early Transcendentals](https://open.umn.edu/opentextbooks/textbooks/415)

[A First Course in Electrical and Computer Engineering](https://open.umn.edu/opentextbooks/textbooks/a-first-course-in-electrical-and-computer-engineering)
