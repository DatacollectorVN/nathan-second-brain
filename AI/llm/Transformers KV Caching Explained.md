---
title: "Transformers KV Caching Explained"
source: "https://medium.com/@joaolages/kv-caching-explained-276520203249"
author:
  - "[[João Lages]]"
published: 2023-10-09
created: 2026-06-17
description: "How caching Key and Value states makes transformers faster"
tags:
  - "clippings"
---
## How caching Key and Value states makes transformers faster

Caching the Key (K) and Value (V) states of generative transformers has been around for a while, but maybe you need to understand what it is exactly, and the great inference speedups that it provides.

The Key and Value states are used for calculating the scaled dot-product attention, as seen in the image below.

![](https://miro.medium.com/v2/resize:fit:1380/format:webp/0*6D_17aytq215gMcF.png)

Scaled dot-product attention and where it is applied in the transformer architecture. (Image source: https://lilianweng.github.io/posts/2018-06-24-attention/#full-architecture )

> ***KV caching occurs during multiple token generation steps and only happens in the decoder*** *(i.e., in decoder-only models like GPT, or in the decoder part of encoder-decoder models like T5). Models like BERT are not generative and therefore do not have KV caching.*

The decoder works in an auto-regressive fashion, as depicted in this GPT-2 text generation example.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*sexO6adGhaKr7aH0.gif)

In the auto-regressive generation of the decoder, given an input the model predicts the next token, and then taking the combined input in the next step the next prediction is made. (Image source: https://jalammar.github.io/illustrated-gpt2/ ).

This auto-regressive behavior repeats some operations, and we can better understand this by zooming in on the masked scaled dot-product attention computation that is calculated in the decoder.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*8xqD4AYTwn6mQXNw0uhDCg.gif)

Step-by-step visualization of the scaled dot-product attention in the decoder. emb\_size means embedding size. Image created by the author.

Since the decoder is causal (i.e., the attention of a token only depends on its preceeding tokens), at each generation step we are recalculating the same previous token attention, when we actually just want to calculate the attention for the new token.

This is where KV comes into play. By caching the previous Keys and Values, we can focus on only calculating the attention for the new token.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*uyuyOW1VBqmF5Gtv225XHQ.gif)

Comparison of scaled dot-product attention with and without KV caching. emb\_size means embedding size. Image created by the author.

==Why== is this optimization important? As seen in the picture above, **the matrices obtained with KV caching are way smaller, which leads to faster matrix multiplications**. The only downside is that it needs more GPU VRAM (or CPU RAM if GPU is not being used) to cache the Key and Value states.

Let’s use [transformers 🤗](https://github.com/huggingface/transformers) to compare the generation speed of GPT-2 with and without KV caching.

```c
import numpy as np
import time
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer

device = "cuda" if torch.cuda.is_available() else "cpu"
tokenizer = AutoTokenizer.from_pretrained("gpt2")
model = AutoModelForCausalLM.from_pretrained("gpt2").to(device)

for use_cache in (True, False):
  times = []
  for _ in range(10):  # measuring 10 generations
    start = time.time()
    model.generate(**tokenizer("What is KV caching?", return_tensors="pt").to(device), use_cache=use_cache, max_new_tokens=1000)
    times.append(time.time() - start)
  print(f"{'with' if use_cache else 'without'} KV caching: {round(np.mean(times), 3)} +- {round(np.std(times), 3)} seconds")
```

On a Google Colab notebook, using a Tesla T4 GPU, these were the reported average and standard deviation times, for generating 1000 new tokens:

> with KV caching: 11.885 +- 0.272 secondswithout KV caching: 56.197 +- 1.855 seconds

The difference in inference speed was huge while the GPU VRAM usage was neglectable, as reported [here](https://discuss.huggingface.co/t/generate-using-k-v-cache-is-faster-but-no-difference-to-memory-usage/31272), so make sure to use KV caching in your transformer model!

Thankfully, this is the default behavior in [transformers 🤗](https://github.com/huggingface/transformers).