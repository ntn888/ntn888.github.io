+++
title = "Setup a development environment"
date = 2022-02-22
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

In this guide we will be setting a development environment for the bl602 development. Neovim is used here but you may choose to your liking. The important requirement is to setup code completion in your editor that'll give ide like features. I find that this is actually useful in the initial phases to aid learning the new framework.

To do this, we are going to setup lsp on neovim. Lsp is native to neovim and only requires some config settings! But you dont have to do this yourself, there's a [github repository](https://github.com/nvim-lua/kickstart.nvim) with preset config file you can just copy. It also helps to configure sane defaults to other settings.

First install neovim. Then you need a language server, kickstarter uses clangd. Install this via your package manager. In ubuntu:
```
sudo apt install clangd
```
Finally copy the kickstarter config. That's it for installation. You may scout around kickstarter to personalise your neovim setup further.

You also need to install a small app called bear. Then in your project directory run:
```
make clean
bear -- make
```

This spits out a file called ```compile_commands.json``` in your project root. Now you will have super ide like features in Neovim that make programming much more pleasurable!

> clangd would throw out an error: ```unknown argument: '-fstrict-volatile-bitfields'```; to avoid this you can do so by instructing clangd to remove that flag from the compile command. To do this create a file ```~/.config/clangd/config.yaml``` with the following contents:
>```
>CompileFlags:
>  Remove: [-fstrict-volatile-bitfields]
>```
