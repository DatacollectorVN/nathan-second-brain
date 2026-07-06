---
title: Attention
tags: [concept]
status: draft
related_papers:
  - "[[Attention Is All You Need]]"
date_added: 2026-06-17
---

## Definition
> A mechanism that maps a query and a set of key-value pairs to an output computed as a weighted sum of the values, where each value's weight comes from a compatibility function between the query and that value's key.

## Intuition
For each position you want to compute, attention asks "which other positions are relevant to me, and how much?" — then blends their value vectors accordingly. Unlike a fixed convolution window or a sequential RNN state, attention can connect any two positions directly, so long-range dependencies are one hop away.

## How It Works
**Scaled dot-product attention** (from [[Attention Is All You Need]]):

`Attention(Q, K, V) = softmax(QKᵀ / √dₖ) · V`

- `Q, K` have dimension `dₖ`; `V` has dimension `dᵥ`.
- The `1/√dₖ` scaling matters: if `q` and `k` have unit-variance components, `q·k` has variance `dₖ`, so without scaling large `dₖ` pushes softmax into tiny-gradient regions.
- Dot-product attention is preferred over additive attention in practice — same theoretical cost but implementable as a single optimized matmul.

**Term-by-term:**
- **Q (queries)** — one vector per position asking "what am I looking for?" Stacked into a matrix, one row per token.
- **K (keys)** — one vector per position advertising "what do I contain?" A query is compared against every key.
- **V (values)** — the actual content each position contributes if attended to. Kept separate from keys so *what you match on* and *what you retrieve* can differ.
- **QKᵀ** — the matrix of dot products between every query and every key: entry $(i,j)$ is the raw **compatibility score** of token $i$ attending to token $j$. Higher = more relevant.
- **÷ √dₖ (scaling)** — divides the scores down so their variance stays ~1 regardless of dimension $d_k$. Without it, large dot products saturate the softmax (near-one-hot), which kills gradients and makes training unstable.
- **softmax(·)** — normalizes each query's row of scores into non-negative **attention weights** that sum to 1. This turns "relevance scores" into a probability distribution over positions to blend.
- **· V (weighted sum)** — multiplies those weights by the value matrix, so each output position is a convex combination of all value vectors, weighted by relevance. This is the actual "blending" step.
- **dₖ vs dᵥ** — keys/queries share dimension $d_k$ (they must, to take dot products); values can have their own dimension $d_v$ (it sets the output width).

**Self-attention** is the case where Q, K, V all come from the same sequence (a layer attending to its own previous layer's outputs). **Cross / encoder-decoder attention** draws queries from one sequence and keys/values from another.


*Scaled dot-product attention — data flow:*

```mermaid
flowchart LR
    Q["Query Q"] --> MM["matmul: Q x K^T"]
    K["Key K"] --> MM
    MM --> SC["scale by 1/sqrt(dk)"]
    SC --> SM["softmax to weights"]
    V["Value V"] --> WV["weighted sum"]
    SM --> WV
    WV --> O["Output"]
```

## Variants & Evolution
- **Multi-Head Attention** — run `h` attention functions in parallel on learned low-dim projections (`dₖ=dᵥ=dmodel/h`), concat, project. Lets the model attend to multiple representation subspaces; in the original Transformer `h=8`.
- **Masked self-attention** — future positions set to −∞ before softmax, preserving auto-regression in decoders.
- **Restricted / local attention** — attend only to a neighborhood of size `r` to tame the O(n²) cost (raised as future work in the original paper).
- Later efficiency variants worth their own notes when they come up: GQA / MQA (KV-head sharing), MLA (latent KV compression), linear / kernelized attention, and SSM hybrids ([[Hymba]], [[Nemotron-H]]).

## Key Papers
- [[Attention Is All You Need]]

## Related Concepts
- [[Positional Encoding]]
- [[Transformer]]

## My Notes
- The O(n²) cost of full attention is the single design pressure that spawns most of the efficiency / long-context literature I track.
- "Attention" here is the umbrella; when MHA/GQA/MLA accumulate enough depth, consider splitting them into dedicated notes per the README's variant-tracking convention.
