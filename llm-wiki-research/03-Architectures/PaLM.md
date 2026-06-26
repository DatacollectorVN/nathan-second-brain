---
title: PaLM
family: PaLM (Pathways Language Model)
tags: [architecture]
status: draft
year: 2022
params: 540B (largest variant; also 8B / 62B)
context_length: 2048
date_added: 2026-06-26
---

## Overview
PaLM (Pathways Language Model) is a dense, decoder-only Transformer LLM from Google (Chowdhery et al., 2022, arXiv:2204.02311), trained with the Pathways system. Its 540B-parameter variant was a frontier model for few-shot reasoning and is the **frozen base model** used in [[ReAct - Synergizing Reasoning and Acting in Language Models|ReAct]].

## Key Design Choices
- Dense decoder-only [[Transformer]] scaled to 540B parameters via the Pathways infrastructure.
- Trained for strong few-shot / in-context learning, including multi-step [[Chain-of-Thought]] reasoning.

## Comparison to Previous
| Feature | This Model | Previous |
|---------|------------|----------|
| Scale | 540B dense params | [[GPT-3]] 175B |
| Infra | Pathways (multi-pod TPU) | Single-cluster training |

## Training Details
- Dataset: large multilingual web/code/book corpus (per PaLM paper).
- Compute: trained across TPU v4 pods via Pathways.
- Techniques: standard autoregressive LM pretraining at scale.

## Strengths & Weaknesses
**Strengths:** strong few-shot reasoning; the scale that makes prompt-only [[ReAct - Synergizing Reasoning and Acting in Language Models|ReAct]] viable.
**Weaknesses:** not openly accessible at ReAct's publication; very large to serve.

## Key Papers
- [[ReAct - Synergizing Reasoning and Acting in Language Models]]

## Related
- [[Transformer]]
- [[GPT-3]]
