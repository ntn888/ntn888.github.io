+++
title = "Zephyr-7b-beta: Or how to run a ChatGPT alternative on an 8GB Graphics Card"
date = 2023-11-14 00:27:00
draft = false

[taxonomies]
categories = ["update"]
tags = ["opensource", "AI", "selfhosted", "llamav2"]

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

Recently we have seen the rise of the small LLMs. As discussed in [this article](@/blog/llama2.md) the [Zephyr-7b-beta](https://huggingface.co/TheBloke/zephyr-7B-beta-GGUF) model is making ground for it's impressive results. Here we shall see how to run the model locally on your system, all you need is a GPU with 8GB VRAM. My [workstation](@/blog/x99_motherboards.md) sports an AMD RX 580 card that is ideally suited for this task! While this guide is targetted towards AMD cards note that you can run it on NVIDIA as well. On my system it runs at speed around 7-9 tokens/sec.

We will use the 'Text generation web UI' [https://github.com/oobabooga/text-generation-webui](https://github.com/oobabooga/text-generation-webui) as our client. This is the software that helps us to host and use the LLM model.

First we need to install what is called ROCm. This is a library that works on top of the AMD GPU driver. The following steps assume you're running POP-OS 22.04. You can follow this [guide](https://github.com/danrauch/HOWTO-PopOS-AMD-HIP-with-Blender). We need to install version 5.6 (as of Nov 2023). The requirement is dictated by 'Text generation web UI', so see its documentation for the exact version you need in future.

> You may wish to install `radeontop`, an app that'll give various deets of the running status of the GPU. And run it alongside in another terminal while you prompt away.

Once ROCm is installed we can proceed with installing 'Text generation web UI'.

First we need to set an environment variable:

```
export ROCBLAS_TENSILE_LIBPATH=/opt/rocm/lib/rocblas/library/
```

>You'll have to run the above evertime before you invoke the 'Text generation web UI' with `./start_linux.sh` or the app will crash when you load in your model.

Finally:

```
git clone https://github.com/oobabooga/text-generation-webui.git
cd text-generation-webui
./start_linux.sh
```

That last command is a convenient script that on first run does the actual installation. It will create the directory `installer_files`. When prompted for the type of installation enter `B` for AMD GPU.

It'll download a couple of GBs and take some time.

Once finished open up the displayed link in your browser. You'll be greeted with the 'Text generation web UI' interface.

The settings will be easily overwhelming at first with lots of variables but I'm going to present here what worked on my 8GB AMD card!

Obviously first we need to download a model. Go to the `Model` tab and enter `TheBloke/zephyr-7B-beta-GGUF` in the `Download Model` field. In the second `File name` box enter: `zephyr-7b-beta.Q4_K_M.gguf` which is recommended for 8GB cards. You can see details in the HuggingFace card [page](https://huggingface.co/TheBloke/zephyr-7B-beta-GGUF).

Once it's downloaded push the reload button next to the model name `blank` on the top and then select the model name. Then set the following options below:

```
n-gpu-layers: anything above 35
n_ctx: 8000
```

Finally press load. This will take some time. Then goto `Parameters` tab. Under `Generation`:

```
max_new_tokens: 2000
top_p: 0.95
top_k: 40
```

Under `Instruction Template`: Choose `ChatML`.

That's it for settings.

Now go to `Default` tab and paste the following prompt:

```
<|system|>
You are a creative writing assistant
<|user|>
<your prompt here>
<|assistant|>
```

Substitue it with your prompt and have fun!


Zephyr-7b brings the excitement of a capable model into the hands of affordable 8Gig GPU cards. Very quickly you'll be amazed at the results, although there is some telling difference from the present benchmark - GPT-4!


But admittedly it a fast moving target. There's new strides being made *every single day*. And many claim it wouldn't be the distant future that a 7b model easily outperfoms the current performance of GPT-4! And I can't wait for that day to self-host it on my humble 8Gig card.
