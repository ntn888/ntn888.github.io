+++
title = "Sphinx Documentation Generator"
date = 2022-12-07
draft = false

[taxonomies]
categories = ["update"]
tags = ["selfhosted", "documentation"]

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

Today I write about the python documentation generator; Sphinx. Which is based on reStructuredText.

reStructuredText is an alternative to Markdown in styling text fragments using simple relevant syntax. However restructured text (or reST for short) is bit more versatile and in my opinion more featured. It is also extensible using third party plugins! One notable feature is the native ability to handle LaTex; which is great for including Mathematic formulas.

In comparison with Markdown, reST aims to be more readable and less obtrusive. For example link addresses are provided with reference as opposed to being inline as in Markdown. The second method is also possible as I said it being versatile.

I intend to document my ideas about RISC-V lowlevel concepts in Sphinx; I believe this will be an interesting take.

One of the initial difficulty in grasping Sphinx is understanding the toctree for linking the table of contents. Which I will clarify here...

Basically it is a recursive link to the other page as a relative reference from the current page's current directory. Once you have generated the base skeleton project from the `sphinx-quickstart` command; you just need to append these links to other pages in the toc section in `index.rst`. Like so:

index.rst:
```
.. toctree::
   :maxdepth: 4
   :caption: Contents:

   fm/welcome
```
And then in `./fm/welcome.rst` for example:
```

=================
Test welcome page
=================

This is the first proper page we will write. This should show up as the first page from the TOC!

.. toctree::
   :maxdepth: 4

   test-page_1
   test-page_2
```


The above welcome page contains reference to `test-page_1.rst` and `test-page_2.rst` which we will need to provide inside the `./fm/` subdir.

As is apparent you can structure your book directory into other subfolders as `./ch1/` and so on.

The Sphinx documentation can be found [here](https://www.sphinx-doc.org/en/master/) and the reST syntax is [here](https://docutils.sourceforge.io/docs/user/rst/quickstart.html).


Happy Writing!
