---
title: Llama-3.1-8B-Instruct
family: Llama (Meta)
tags: [architecture, SLM, open-source]
status: stable
year: 2024
params: 8B
context_length: 128K
date_added: 2025-06-13
---

## Overview
Llama-3.1-8B-Instruct is Meta's 8-billion parameter instruction-tuned model, the most widely adopted "small" open model for research and production. It does **not** natively have long CoT reasoning ability — it's a strong general-purpose model that researchers commonly use as the **target** of reasoning distillation experiments.

## Key Design Choices
- **Dense Transformer** (not MoE)
- **GQA** (Grouped-Query Attention) for efficient inference
- **128K context** (up from 8K in Llama-2)
- **RoPE** with frequency scaling
- Instruction-tuned via multi-stage SFT + DPO + rejection sampling

## Comparison to Previous
| Feature | Llama-3.1-8B | Llama-2-7B | Llama-3-8B |
|---------|--------------|------------|------------|
| Params | 8B | 7B | 8B |
| Context | 128K | 4K | 8K |
| Tokenizer | 128K vocab | 32K vocab | 128K vocab |
| Training tokens | 15T+ | 2T | 15T |
| Reasoning | Standard | Weak | Standard |

## Training Details
- **Dataset**: 15T+ tokens, heavily filtered and curated
- **Compute**: Tens of thousands of H100 GPUs
- **Techniques**:
  - Supervised fine-tuning on instruction data
  - DPO and rejection sampling for alignment
  - Code data heavy ratio for reasoning emergence

## Strengths & Weaknesses
**Strengths:**
- Excellent general-purpose baseline
- 128K context is generous for an 8B model
- Strong tool-use and instruction-following
- Widely supported by inference engines (vLLM, llama.cpp, etc.)

**Weaknesses:**
- No native long-CoT reasoning — needs distillation/fine-tuning
- Math performance ~37% on MATH (vs Qwen2.5-7B at 59%)
- Smaller models in the same family (1B, 3B) are notably weaker

## Use in Reasoning Distillation Research
From [[Efficient Long CoT Reasoning in Small Language Models]]:
- **Base accuracy**: 37.80% on MATH, 12.97% on AIME
- After full CoT distillation: 54.80% MATH, 16.51% AIME (but 7262 tokens on AIME)
- After efficient distillation: 52.40% MATH, 17.90% AIME with **47% fewer tokens**

## Key Papers
- Meta AI 2024 — *The Llama 3 Herd of Models*
- [[Efficient Long CoT Reasoning in Small Language Models]] — uses Llama-3.1-8B as one of two target SLMs

## Related
- [[Small Language Models]]
- [[Qwen2.5-7B-Instruct]]
- [[Knowledge Distillation]]

## My Notes
Llama-3.1-8B is my likely default target for SLM experiments at RMIT. It has weaker math performance than Qwen-2.5-7B but is more general-purpose, better instruction following, and has stronger community tooling support.
