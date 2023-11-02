+++
title = "Octave Command Line Mode"
date = 2022-02-27
draft = false

[taxonomies]
categories = ["update"]
tags = ["math", "MATLAB", "open-source"]

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

GNU Octave has a lesser known command line mode. I think it's awesome. You get to work in the always familiar environment in the shell with vim running alongside to edit script files... You have access to certain shell commands such as ```cd```, ```ls```, ```mkdir```. Enough to navigate you way around. Makes you feel like a true hacker rather than working in the outdated, ugly GUI IDE.
Here's a snapshot:


```
> octave
GNU Octave, version 6.1.1~hg.2021.01.26
Copyright (C) 2020 The Octave Project Developers.
This is free software; see the source code for copying conditions.
There is ABSOLUTELY NO WARRANTY; not even for MERCHANTABILITY or
FITNESS FOR A PARTICULAR PURPOSE.  For details, type 'warranty'.

Octave was configured for "x86_64-pc-linux-gnu".

Additional information about Octave is available at https://www.octave.org.

Please contribute if you find this software useful.
For more information, visit https://www.octave.org/get-involved.html

Read https://www.octave.org/bugs.html to learn how to submit bug reports.
For information about changes from previous versions, type 'news'.

octave:1> pi
ans = 3.1416
octave:2> 
```
