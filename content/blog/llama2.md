+++
title = "Checking out a free opensource ChatGPT alternative: Zephry-7b-Beta"
date = 2023-11-12 22:27:00
draft = false

[taxonomies]
categories = ["update"]
tags = ["opensource", "AI", "selfhosted"]

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

In a previous [article](@/blog/llama.md) we reviewed the free LLM Llama2, the 70b version. This is prohibitively large to run on your local machines as it requires around 48GB of GPU VRAM. I instead tried the 13b version but it was still too large for [my GPU](@/blog/x99_motherboards.md) with only 8Gigs of VRAM. So ran it on my CPU instead and it was painfully slow.. Here we will look at a viable alternative: Zephry-7b-Beta, the latest sensation in the world of natural language processing!

Zephry-7b-Beta is a groundbreaking large language model that has been capturing the attention of the scientific community due to its exceptional abilities in natural language processing, despite being a relatively small model. This innovative language model, developed by the research wing of [HuggingFace](https://huggingface.co/), boasts impressive feats that challenge the traditional notion that larger models are better. Although Zephry-7b-Beta has just over 7 billion parameters, in many benchmarks, outperforms larger models like GPT-3 and BERT-Large, which have more than 175 billion parameters each. This remarkable performance has been attributed to the model's unique architecture and training methods, which have allowed it to achieve a balance between accuracy and efficiency, making it an ideal candidate for real-world applications where resource constraints can pose a challenge. As in the case of hosting on a home PC.

It gives out an impressive interactive experience. And I found that it resembled GPT4 more than the Llama 2 model.

I first sought to install it locally as this is reported to run comfortably on 8G GPUs. So I downloaded the prerequisite library ROCm (for driving the AMD GPU), and then installed [`Text generation web UI`](https://github.com/oobabooga/text-generation-webui). Downloaded the LLM model from [HuggingFace site](https://huggingface.co/TheBloke/zephyr-7B-beta-GGUF). Some of the troubleshooting steps I took can be seen in this [thread](https://github.com/oobabooga/text-generation-webui/issues/4558). But again it was too slow to be usable.. Maybe I'm doing something wrong[^1].

Then I resorted to hosting on a cloud GPU provider. I made an account at [vast.ai](https://vast.ai/) (these folks being the cheapest). I made an RTX A6000 (48GB VRAM) instance with 8G disk space. Downloaded the same model image (the Q5_K_M variant). Then I made parameter choices in `Text generation web UI` (not necessarily optimum, but sure something to play around with and test):

- Set Generation/Preset to debug-deterministic
- Increase max new tokens to 2000
- ChatML for Instruction template

Also remember to set `n-gpu-layers` to over 35 for this gguf model when loading it.

>There are plenty of Youtube videos on how to use `Text generation web UI`.


And used the following prompt (copied from the model page and tweaked):

```
<|system|>
You are a creative writing assistant
<|user|>
<your prompt here>
<|assistant|>
```

I have to say.. The results were snappy. And like I said earlier the responses were impressive.


Although I have been using the Default/Notebook input method of the `Text generation web UI`, I'm looking to study the chat prompt. And see if it gives a more fluid interactive experience.

This has been my log of trying out a free new ChatGPT alternative. Although I didn't know why it didn't run well on my local system since this is most suited for this task. Let me know what your goto model is these days!

[^1]: I've finally managed to run it. See [this post](@/blog/llama-howto.md) for updated info.

