---
title: Qwen2.5-7B-Instruct
family: Qwen (Alibaba)
tags: [architecture, SLM, open-source]
status: stable
year: 2024
params: 7B
context_length: 128K
date_added: 2025-06-13
---

## Overview
Qwen2.5-7B-Instruct is Alibaba's instruction-tuned 7B model, notable for being unusually **strong at math and code** for its size — often outperforming larger models on reasoning benchmarks. It is one of the two reference SLMs used in [[Efficient Long CoT Reasoning in Small Language Models]].

## Key Design Choices
- **Dense Transformer**
- **GQA** (Grouped-Query Attention)
- **RoPE** with extended context (128K via YaRN)
- Tokenizer optimized for Chinese + English + code
- Extensive **math and code pretraining mix** (key differentiator)

## Comparison to Previous
| Feature | Qwen2.5-7B | Qwen2-7B | Llama-3.1-8B |
|---------|------------|----------|--------------|
| Params | 7B | 7B | 8B |
| Pretraining tokens | 18T | 7T | 15T+ |
| Math (MATH) | ~59% | ~50% | ~38% |
| Code (HumanEval) | ~85% | ~80% | ~72% |
| Context | 128K | 128K | 128K |

## Training Details
- **Dataset**: 18T tokens with heavy math/code emphasis
- **Compute**: Alibaba Cloud H800 clusters
- **Techniques**:
  - Multi-stage SFT
  - DPO for alignment
  - Specialized math/code data filtering pipeline

## Strengths & Weaknesses
**Strengths:**
- Best-in-class math and code reasoning for 7B size
- Strong base for further distillation/fine-tuning
- Good multilingual coverage (CJK + EN)

**Weaknesses:**
- Slightly weaker on general conversational tasks vs Llama-3.1
- Western community tooling lags Llama ecosystem
- Distillation can sometimes regress base performance (observed in [[Efficient Long CoT Reasoning in Small Language Models]] where FCS distillation tanked GSM8K from 83.7% → 56.79%)

## Use in Reasoning Distillation Research
From [[Efficient Long CoT Reasoning in Small Language Models]]:
- **Base accuracy**: 59.40% on MATH, 21.76% on AIME (already strong!)
- After efficient distillation (SFT+DPO): 56.60% MATH, 21.54% AIME with **only 489 tokens** on MATH vs full CoT's 2712
- Achieves **82% token reduction** on MATH with minimal accuracy loss

## Key Papers
- Qwen Team 2024 — *Qwen2.5 Technical Report*
- [[Efficient Long CoT Reasoning in Small Language Models]] — uses Qwen2.5-7B as target SLM

## Related
- [[Small Language Models]]
- [[Llama-3.1-8B-Instruct]]
- [[Knowledge Distillation]]

## My Notes
For pure math/reasoning SLM research, Qwen2.5-7B is often a better base than Llama-3.1-8B. The Efficient Long CoT paper showed Qwen achieves more aggressive token reduction (50.75% remaining) than Llama (61.83% remaining) — likely because Qwen's stronger base reasoning means it needs less of the teacher's CoT to derive the answer.
