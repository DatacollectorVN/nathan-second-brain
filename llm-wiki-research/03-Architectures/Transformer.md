---
title: Transformer
family: Transformer
tags: [architecture]
status: draft
year: 2017
params: "base 65M / big 213M (original NMT models)"
context_length: "n/a (sentence-level; positional encoding extends to longer in principle)"
date_added: 2026-06-17
---

## Overview
The Transformer is the encoder-decoder sequence-transduction architecture introduced in [[Attention Is All You Need]]. It replaces recurrence and convolution entirely with stacked [[Attention|self-attention]] and position-wise feed-forward layers. It matters because it is the architectural foundation of essentially all modern LLMs/SLMs — decoder-only variants (GPT-family, [[Llama-3.1-8B-Instruct|Llama]], [[Qwen2.5-7B-Instruct|Qwen]], [[Phi]], [[SmolLM2]]) descend directly from it.

## Key Design Choices
- **Pure attention, no recurrence/convolution** — every position attends to every other in O(1) sequential steps, enabling within-example parallelism.
- **Residual + LayerNorm around every sub-layer** — `LayerNorm(x + Sublayer(x))`; all sub-layers and embeddings emit `dmodel = 512` (base).
- **Multi-head attention** (`h=8`, `dₖ=dᵥ=64`) for attending to multiple representation subspaces at once.
- **Masked decoder self-attention** to preserve the auto-regressive property.
- **Sinusoidal [[Positional Encoding]]** added to token embeddings (no inherent order otherwise).
- **Weight tying** between the two embedding layers and the pre-softmax linear projection (embedding weights scaled by `√dmodel`).

## Comparison to Previous
| Feature | Transformer | RNN / CNN seq2seq |
|---|---|---|
| Sequential ops per layer | O(1) | O(n) (RNN) / O(1) (CNN) |
| Complexity per layer | O(n²·d) | O(n·d²) (RNN) / O(k·n·d²) (CNN) |
| Max path between positions | O(1) | O(n) (RNN) / O(log_k n) (CNN) |
| Parallelism within example | High | Low (RNN) |

## Training Details
- **Dataset:** WMT 2014 EN-DE (~4.5M pairs, ~37k shared BPE) and EN-FR (36M sentences, 32k word-piece).
- **Compute:** 8× NVIDIA P100. Base ≈ 12 h / 100k steps; Big ≈ 3.5 days / 300k steps.
- **Techniques:** Adam (`β₁=0.9, β₂=0.98`), warmup-then-inverse-sqrt LR schedule (4000 warmup steps), residual dropout (0.1 base / 0.3 big EN-DE), label smoothing `ε_ls=0.1`.

## Strengths & Weaknesses
**Strengths:** Massively parallelizable training; constant-path long-range dependency modeling; strong translation + parsing results at a fraction of prior training cost; architecturally simple and scalable.
**Weaknesses:** O(n²) attention cost in sequence length (the seed of the entire efficiency literature — see [[Hymba]], [[Nemotron-H]]); auto-regressive decoding remains sequential at inference; no built-in notion of position without explicit encodings.

## Key Papers
- [[Attention Is All You Need]]

## Related
- [[Attention]]
- [[Positional Encoding]]
- [[Small Language Models]]
