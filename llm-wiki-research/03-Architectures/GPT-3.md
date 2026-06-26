---
title: GPT-3
family: GPT
tags: [architecture]
status: draft
year: 2020
params: 175B
context_length: 2048
date_added: 2026-06-26
---

## Overview
GPT-3 is OpenAI's 175B-parameter decoder-only Transformer (Brown et al., 2020), the model that popularized large-scale **few-shot in-context learning**. The instruction-tuned `text-davinci-002` variant is used in [[ReAct - Synergizing Reasoning and Acting in Language Models|ReAct]]'s Appendix A.1.

## Key Design Choices
- Dense decoder-only [[Transformer]] at 175B parameters.
- Few-shot / zero-shot task solving purely via prompting, no task-specific finetuning.

## Comparison to Previous
| Feature | This Model | Previous |
|---------|------------|----------|
| Scale | 175B params | GPT-2 1.5B |
| Paradigm | In-context few-shot | Finetune-per-task |

## Training Details
- Dataset: filtered Common Crawl + WebText2 + books + Wikipedia.
- Compute: large-scale autoregressive pretraining.
- Techniques: next-token LM objective; later instruction-tuned variants (e.g. `text-davinci-002`).

## Strengths & Weaknesses
**Strengths:** strong general few-shot performance; in ReAct, `text-davinci-002` reportedly beat PaLM-540B on HotpotQA/ALFWorld (Appendix A.1).
**Weaknesses:** closed weights; 2048-token context limits long agent trajectories.

## Key Papers
- [[ReAct - Synergizing Reasoning and Acting in Language Models]]

## Related
- [[Transformer]]
- [[PaLM]]
