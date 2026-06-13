---
title: DeepSeek-R1
family: DeepSeek
tags: [architecture, reasoning-model, open-source]
status: stable
year: 2025
params: 671B (MoE, 37B active) + distilled variants 1.5B-70B
context_length: 128K
date_added: 2025-06-13
---

## Overview
DeepSeek-R1 is an open-source large reasoning model from DeepSeek that matches or exceeds OpenAI's o1 on math and coding benchmarks. It was a watershed moment in 2025 — proving that long chain-of-thought reasoning could be replicated openly via RL training, and that the capability could be **distilled** into smaller models.

## Key Design Choices
- **Mixture of Experts (MoE)** architecture — 671B total parameters, only 37B active per forward pass
- **Long Chain-of-Thought** wrapped in `<think>...</think>` tags
- **Pure RL training** (DeepSeek-R1-Zero) — no SFT cold start needed
- **R1 variant** adds multi-stage SFT + RL for better instruction following
- **Distilled SLM family** — 1.5B, 7B, 8B, 14B, 32B, 70B variants released alongside

## Comparison to Previous
| Feature | DeepSeek-R1 | OpenAI o1 | Standard LLMs (GPT-4) |
|---------|-------------|-----------|----------------------|
| Open weights | ✅ Yes | ❌ No | ❌ No |
| Reasoning visible | ✅ Yes (in `<think>` tags) | ❌ Hidden | N/A (no explicit CoT) |
| Architecture | MoE | Unknown | Dense (mostly) |
| Training | Pure RL + multi-stage | RL + secret sauce | Pretraining + RLHF |
| Distilled variants | ✅ Released | ❌ | ❌ |

## Training Details
- **Dataset**: math, code, and reasoning-rich data
- **Compute**: trained on H800 GPUs (export-restricted variant of H100)
- **Techniques**:
  - GRPO (Group Relative Policy Optimization) instead of PPO
  - Rule-based rewards for math (verifiable correctness)
  - Multi-stage pipeline: SFT cold start → RL → SFT → RL again
  - Self-evolution: model improves its own reasoning patterns during RL

## Strengths & Weaknesses
**Strengths:**
- Strong math reasoning (competitive with o1 on AIME, MATH)
- Open weights enable research and customization
- Distilled variants make reasoning accessible to SLM users
- Reasoning traces are inspectable

**Weaknesses:**
- **Overthinking problem** — generates excessive redundant steps (see [[Overthinking in Reasoning Models]])
- Long CoT inflates inference cost
- Distilled SLMs inherit the verbosity issue → motivated [[Efficient Long CoT Reasoning in Small Language Models]]
- General chat / instruction following slightly weaker than top closed models
- Some training details opaque (data curation)

## Distilled Variants (used in many downstream papers)
- DeepSeek-R1-Distill-Qwen-1.5B
- DeepSeek-R1-Distill-Qwen-7B
- DeepSeek-R1-Distill-Llama-8B
- DeepSeek-R1-Distill-Qwen-14B
- DeepSeek-R1-Distill-Qwen-32B
- DeepSeek-R1-Distill-Llama-70B

These were created by fine-tuning open SLMs on R1-generated reasoning traces (standard distillation, no RL).

## Key Papers
- Guo et al. 2025 — *DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning*
- [[Efficient Long CoT Reasoning in Small Language Models]] — critiques and improves distillation pipeline

## Related
- [[Chain-of-Thought]]
- [[Knowledge Distillation]]
- [[Overthinking in Reasoning Models]]
- [[Small Language Models]]

## My Notes
R1 is the **reference point** for open reasoning models. For SLM research, the distilled variants are gold — they let you study reasoning behavior at 7B scale without training from scratch. Most efficient-reasoning papers in 2025 use R1 traces as the starting dataset (e.g., OpenR1-Math-220k).
