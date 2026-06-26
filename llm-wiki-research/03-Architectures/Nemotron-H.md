---
title: Nemotron-H
family: NVIDIA Nemotron
tags: [architecture, SLM, hybrid, mamba, open-source]
status: draft
year: 2025
params: 2B / 4.8B / 9B (hybrid Mamba-Transformer)
context_length: long (see primary paper)
date_added: 2026-06-16
---

## Overview
Nemotron-H is NVIDIA's family of hybrid **Mamba-Transformer** small language models (2B, 4.8B, 9B). Per [[Small Language Models are the Future of Agentic AI]], they achieve instruction-following and code-generation accuracy comparable to **dense 30B LLMs** of the same generation at roughly an **order-of-magnitude fewer inference FLOPs**.

## Key Design Choices
- Hybrid architecture mixing Mamba (state-space) layers with Transformer attention layers
- Tuned for high throughput / low FLOPs at small scale
- Positioned for agentic deployment where efficiency per call dominates

## Comparison to Previous
| Feature | Nemotron-H | Dense 30B LLM (same gen) |
|---------|------------|--------------------------|
| Params | 2–9B | ~30B |
| Inference FLOPs | ~10× fewer | baseline |
| Instruction following / code-gen | comparable | baseline |

## Training Details
- Dataset: see NVIDIA primary report
- Compute: NVIDIA
- Techniques: hybrid Mamba-attention design

## Strengths & Weaknesses
**Strengths:** strong accuracy-per-FLOP; agent-suitable sizes; hybrid efficiency.
**Weaknesses:** stub — verify exact benchmark numbers and context length against the primary paper before citing.

## Key Papers
- [[Small Language Models are the Future of Agentic AI]] (cites it as evidence SLMs suffice for agents)
- NVIDIA Nemotron-H technical report (primary — to read)

## Related
- [[Small Language Models]]
- [[Hymba]]
- [[Agentic AI]]
