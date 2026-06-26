---
title: Hymba-1.5B
family: NVIDIA Hymba
tags: [architecture, SLM, hybrid, mamba, attention, open-source]
status: draft
year: 2024
params: 1.5B
context_length: see primary paper
date_added: 2026-06-16
---

## Overview
Hymba-1.5B is an NVIDIA **Mamba-attention hybrid-head** small language model. Per [[Small Language Models are the Future of Agentic AI]], it shows best-in-class instruction accuracy and **3.5× greater token throughput** than comparably-sized transformer models, and on instruction following it **outperforms larger 13B models**.

## Key Design Choices
- Hybrid-head design combining Mamba (state-space) and attention heads within layers
- Optimized for throughput at the ~1.5B scale (edge / on-device candidate)

## Comparison to Previous
| Feature | Hymba-1.5B | Comparable Transformer SLM | 13B model |
|---------|-----------|----------------------------|-----------|
| Throughput | 3.5× | baseline | — |
| Instruction following | best-in-class | lower | outperformed by Hymba |

## Training Details
- Dataset / compute / techniques: see NVIDIA Hymba primary report

## Strengths & Weaknesses
**Strengths:** excellent throughput and instruction accuracy at tiny scale; strong edge-deployment fit.
**Weaknesses:** stub — confirm context length and benchmark suite from the primary paper.

## Key Papers
- [[Small Language Models are the Future of Agentic AI]]
- NVIDIA Hymba technical report (primary — to read)

## Related
- [[Small Language Models]]
- [[Nemotron-H]]
- [[Agentic AI]]
