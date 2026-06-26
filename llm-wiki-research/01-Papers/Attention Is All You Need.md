---
title: Attention Is All You Need
authors: Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Łukasz Kaiser, Illia Polosukhin
year: 2017
arxiv: https://arxiv.org/abs/1706.03762
tags:
  - paper
  - attention
  - transformer
  - efficiency
status: needs-review
topic:
  - Transformer Architecture
  - Self-Attention
  - Sequence Transduction
  - Machine Translation
related_concepts:
  - "[[Attention]]"
  - "[[Positional Encoding]]"
  - "[[Transformer]]"
date_added: 2026-06-17
---

## Summary
The dominant sequence-transduction models in 2017 were RNN/CNN encoder-decoders, often augmented with attention. Their inherently sequential computation (each hidden state depends on the previous one) blocks parallelization within a training example and makes long-range dependencies hard to learn. This paper proposes the **Transformer**, an architecture that drops recurrence and convolution entirely and relies *solely* on attention to relate positions. The payoff is twofold: every pair of positions is connected by a constant number of operations (vs. O(n) for RNNs), and the bulk of computation parallelizes across positions. On WMT 2014 the big model reaches **28.4 BLEU EN-DE** and **41.8 BLEU EN-FR** (new single-model SOTA), trained in **3.5 days on 8 P100 GPUs** — a small fraction of the cost of prior SOTA — and generalizes to English constituency parsing.

---

## Key Contributions
- Introduced the **Transformer**: the first sequence-transduction model built entirely on self-attention, with no recurrence and no convolution.
- **Scaled Dot-Product Attention** — `softmax(QKᵀ/√dₖ)V`. The `1/√dₖ` scaling counteracts the dot products growing large in magnitude (variance grows with `dₖ`), which otherwise pushes softmax into low-gradient regions.
- **Multi-Head Attention** — `h=8` parallel attention heads with reduced per-head dimension (`dₖ=dᵥ=dmodel/h=64`), letting the model attend to information from different representation subspaces; total cost stays comparable to single full-dimension attention.
- **Sinusoidal positional encodings** — fixed `sin`/`cos` of geometrically-spaced frequencies, added to embeddings, chosen partly for the hope of extrapolating to longer sequences than seen in training.
- Demonstrated large **parallelization + training-cost** wins while setting new translation SOTA, plus strong transfer to constituency parsing.

---

## Architecture / Method

### Overall shape (encoder–decoder)
```
Inputs → Embedding (+ Positional Encoding) → [Encoder layer] × N=6 → memory
Outputs(shifted right) → Embedding (+ PosEnc) → [Decoder layer] × N=6 → Linear → Softmax → probs
```
- **Encoder layer** (2 sub-layers): multi-head self-attention → position-wise FFN. Each sub-layer wrapped in a residual connection + layer norm: `LayerNorm(x + Sublayer(x))`.
- **Decoder layer** (3 sub-layers): *masked* multi-head self-attention → encoder-decoder (cross) attention → FFN. Masking sets illegal (future) positions to −∞ before softmax, preserving the auto-regressive property.
- All sub-layers and embeddings output `dmodel = 512`.

### Attention
- **Scaled dot-product:** `Attention(Q,K,V) = softmax(QKᵀ/√dₖ)V`. Chosen over additive attention because it's far faster/more memory-efficient in practice (optimized matmul), and scaling closes the quality gap at large `dₖ`.
- **Multi-head:** `MultiHead(Q,K,V) = Concat(head₁..head_h)Wᴼ`, `headᵢ = Attention(QWᵢQ, KWᵢK, VWᵢV)`.
- **Three uses of attention in the model:**
  1. *Encoder-decoder attention* — queries from the previous decoder layer, keys/values from the encoder output (every decoder position can see all input positions).
  2. *Encoder self-attention* — Q/K/V all from the previous encoder layer.
  3. *Decoder self-attention* — masked so each position attends only to positions ≤ itself.

### Position-wise FFN
`FFN(x) = max(0, xW₁ + b₁)W₂ + b₂` — applied identically per position; inner dimension `d_ff = 2048` (≈ two kernel-size-1 convolutions).

### Positional Encoding
`PE(pos,2i) = sin(pos / 10000^(2i/dmodel))`, `PE(pos,2i+1) = cos(pos / 10000^(2i/dmodel))`. Wavelengths form a geometric progression from `2π` to `10000·2π`. Learned positional embeddings gave near-identical results (Table 3 row E); sinusoids were preferred for possible length extrapolation.

### Why self-attention (complexity comparison, Table 1)
| Layer type | Complexity / layer | Sequential ops | Max path length |
|---|---|---|---|
| Self-Attention | O(n²·d) | O(1) | O(1) |
| Recurrent | O(n·d²) | O(n) | O(n) |
| Convolutional | O(k·n·d²) | O(1) | O(log_k(n)) |
| Self-Attention (restricted) | O(r·n·d) | O(1) | O(n/r) |

Self-attention is cheaper than recurrent when `n < d` (typical for sentence-level NMT) and gives constant-length paths between any two positions, which eases learning long-range dependencies.

### Training setup
- Data: WMT 2014 EN-DE (~4.5M sentence pairs, ~37k shared BPE vocab); EN-FR (36M sentences, 32k word-piece vocab). Batches ≈ 25k source + 25k target tokens.
- Hardware/schedule: 8× NVIDIA P100. Base = 100k steps / ~12 h (≈0.4 s/step); Big = 300k steps / ~3.5 days (≈1.0 s/step).
- Optimizer: Adam, `β₁=0.9, β₂=0.98, ε=10⁻⁹`; LR warmup over 4000 steps then inverse-sqrt decay.
- Regularization: residual dropout `P_drop=0.1` (base); label smoothing `ε_ls=0.1` (hurts perplexity, helps accuracy/BLEU).
- Inference: beam size 4, length penalty `α=0.6`; checkpoint averaging (last 5 base / last 20 big).

### Hyperparameters: base vs big
| | N | dmodel | d_ff | h | dₖ=dᵥ | P_drop | steps | params |
|---|---|---|---|---|---|---|---|---|
| Base | 6 | 512 | 2048 | 8 | 64 | 0.1 | 100K | 65M |
| Big | 6 | 1024 | 4096 | 16 | 64 | 0.3 | 300K | 213M |

---

## Results & Benchmarks

### Machine translation (WMT 2014 newstest2014, Table 2)
| Benchmark | Score | Baseline |
|---|---|---|
| EN-DE BLEU (Transformer big) | 28.4 | prev. best incl. ensembles, beaten by >2.0 BLEU |
| EN-DE BLEU (Transformer base) | 27.3 | surpasses all prior published single models |
| EN-FR BLEU (Transformer big) | 41.8 | new single-model SOTA |
| EN-FR BLEU (Transformer base) | 38.1 | — |
| Training cost, big (FLOPs) | 2.3·10¹⁹ | < 1/4 of prior EN-FR SOTA |
| Training cost, base (FLOPs) | 3.3·10¹⁸ | — |

> [!note] Internal inconsistency in the source
> The abstract **and Table 2** report **41.8** BLEU EN-FR for the big model, but the prose in §6.1 says **41.0**. The headline number used everywhere else in the paper is 41.8; the 41.0 in §6.1 appears to be a typo. Recorded here as 41.8 with the discrepancy flagged.

### English constituency parsing (WSJ §23 F1, Table 4)
| Setting | Transformer (4-layer, dmodel=1024) F1 |
|---|---|
| WSJ-only, discriminative | 91.3 |
| Semi-supervised (~17M sentences) | 92.7 |

Outperforms all prior reported models except the Recurrent Neural Network Grammar — strong for a model with almost no task-specific tuning, and notably beats the BerkeleyParser even in the 40k-sentence WSJ-only regime.

### Ablations (Table 3, dev set newstest2013)
- Single-head attention is ~0.9 BLEU worse than the best head count; too many heads also degrades quality.
- Reducing `dₖ` hurts — suggests dot-product compatibility is not trivially easy.
- Bigger models help; dropout is important against overfitting.
- Learned vs sinusoidal positional encoding ≈ identical (25.7 vs 25.8 BLEU).

---

## Limitations
- Self-attention is **O(n²·d)** in sequence length — quadratic memory/compute makes very long sequences (long documents, audio, images, video) expensive; the paper only gestures at *restricted* attention (neighborhood `r`) as future work.
- Evaluated almost entirely on **sentence-level machine translation** (short `n`, where `n<d` makes self-attention favorable) plus one parsing task — the favorable complexity story is partly a function of that regime.
- The headline EN-FR number is internally inconsistent in the paper (41.8 vs 41.0; see note above).
- Fixed sinusoidal length-extrapolation is **hypothesized**, not demonstrated, in the paper.
- No analysis of inference latency in the auto-regressive decoding regime (decoding is still sequential over output positions, even though training parallelizes).

---

## My Notes & Questions
- This is the foundational substrate for essentially everything else in this wiki — [[Small Language Models]], [[DeepSeek-R1]], [[Chain-of-Thought]] reasoning, agentic stacks. Worth treating [[Transformer]] and [[Attention]] as the root nodes of the graph.
- The O(n²) cost flagged as a "future work" footnote here is the exact pressure that later drives the whole efficiency literature I care about: [[Hymba]], [[Nemotron-H]], linear-attention / SSM hybrids, KV-cache work. Useful to anchor those notes back to this paragraph.
- The `1/√dₖ` scaling is a tiny detail with outsized importance — it's a variance-control trick (dot product of two `dₖ`-dim unit-variance vectors has variance `dₖ`). Same family of reasoning shows up later in init/normalization schemes.
- Open question for my own work: the decoder is still **sequential at inference**. The training-time parallelism win does not remove auto-regressive decoding latency — which is precisely why on-device SLM inference cares about KV cache and speculative decoding. Good bridge to my on-device research thread.
- Practical: arXiv v7 (2023) adds the Google reproduction-permission banner and minor fixes; the architecture is unchanged from the 2017 NeurIPS version.

---

## Related
- [[Transformer]]
- [[Attention]]
- [[Positional Encoding]]
- [[Small Language Models]]
- [[Chain-of-Thought]]
