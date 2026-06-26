---
title: SmolLM2
family: HuggingFace SmolLM
tags: [architecture, SLM, open-source]
status: draft
year: 2024
params: 125M / 360M / 1.7B
context_length: see primary paper
date_added: 2026-06-16
---

## Overview
SmolLM2 is HuggingFace's family of compact language models (125M–1.7B parameters). Per [[Small Language Models are the Future of Agentic AI]], each model matches the **language understanding, tool calling, and instruction following** of 14B contemporaries, and rivals **70B models from two years prior**.

## Key Design Choices
- Heavily curated training data (the SmolLM thesis: data quality over scale)
- Tiny sizes targeting on-device and cheap fine-tuning

## Comparison to Previous
| Feature | SmolLM2 (≤1.7B) | 14B contemporary | 70B (2 yrs prior) |
|---------|-----------------|------------------|-------------------|
| Lang. understanding / tool calling / instruction following | comparable | baseline | rivaled |

## Training Details
- Dataset: curated corpora (see HuggingFace SmolLM2 report)
- Techniques: data-centric training

## Strengths & Weaknesses
**Strengths:** strong capability-per-param; fully open; ideal for edge/agent specialists.
**Weaknesses:** stub — verify exact benchmark coverage from the primary report.

## Key Papers
- [[Small Language Models are the Future of Agentic AI]]
- HuggingFace SmolLM2 report (primary — to read)

## Related
- [[Small Language Models]]
- [[Phi]]
- [[Agentic AI]]
