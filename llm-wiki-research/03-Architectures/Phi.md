---
title: Phi (Microsoft Phi series)
family: Microsoft Phi
tags: [architecture, SLM, data-centric, open-source]
status: draft
year: 2023-2024
params: Phi-2 2.7B / Phi-3-small 7B (series spans 1.3B–14B)
context_length: up to 128K (Phi-3 variants)
date_added: 2026-06-16
---

## Overview
The Microsoft **Phi** series pioneered the "textbook-quality data" approach to small language models. Per [[Small Language Models are the Future of Agentic AI]]: **Phi-2 (2.7B)** reaches commonsense-reasoning and code-generation scores on par with **30B models while running ~15× faster**; **Phi-3-small (7B)** matches 70B-class language understanding/commonsense and approaches their code-generation.

## Key Design Choices
- **Data-centric training** — synthetic "textbook-quality" + heavily filtered web data over raw scale
- Compact dense Transformers tuned for capability-per-parameter
- Later variants (Phi-3, Phi-4) extend context and add long-context support

## Comparison to Previous
| Model | Params | Claim | Reference |
|-------|--------|-------|-----------|
| Phi-2 | 2.7B | commonsense + code ≈ 30B, ~15× faster | 30B same-gen |
| Phi-3-small | 7B | understanding/commonsense ≈ 70B; code near 70B | 70B same-gen |

## Training Details
- Dataset: curated + synthetic "textbook-quality" data
- Techniques: data filtering/synthesis as the primary lever

## Strengths & Weaknesses
**Strengths:** demonstrated that data quality can substitute for scale; strong agent-relevant skills at small size.
**Weaknesses:** stub — benchmark specifics vary by Phi generation; check the relevant technical report.

## Key Papers
- [[Small Language Models are the Future of Agentic AI]]
- Phi-2 / Phi-3 technical reports (primary — to read)

## Related
- [[Small Language Models]]
- [[SmolLM2]]
- [[Agentic AI]]
