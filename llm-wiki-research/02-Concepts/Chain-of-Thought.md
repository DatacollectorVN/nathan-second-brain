---
title: Chain-of-Thought (CoT)
tags: [concept, reasoning, prompting]
status: stable
related_papers:
  - "[[Efficient Long CoT Reasoning in Small Language Models]]"
date_added: 2025-06-13
---

## Definition
Chain-of-Thought (CoT) is a prompting and training technique where a model generates intermediate reasoning steps before producing the final answer, rather than answering directly.

## Intuition
Instead of asking a model "what's 47 × 23?" and getting an answer, you let it "think out loud" — break it into smaller pieces, solve each, then combine. Like showing your work on a math test. This dramatically improves performance on complex reasoning tasks.

## How It Works
- **Prompt-based CoT**: Add phrases like *"Let's think step by step"* (Kojima et al., 2022) to elicit reasoning
- **Training-based CoT**: Fine-tune models on data that explicitly contains reasoning traces
- The model generates a sequence of reasoning steps `[s1, s2, ..., sn]` before the final answer
- In modern reasoning models (DeepSeek-R1, o1, QwQ), the reasoning is wrapped in `<think>...</think>` tags

## Variants & Evolution
| Era | Variant | Key Idea |
|-----|---------|----------|
| 2022 | **Standard CoT** (Wei et al.) | "Let's think step by step" prompting |
| 2022 | **Zero-shot CoT** (Kojima et al.) | No exemplars needed, just trigger phrase |
| 2023 | **Self-Consistency** (Wang et al.) | Sample multiple CoTs, majority vote |
| 2024 | **Long CoT / Test-time Scaling** (o1, R1) | Scale reasoning length to improve accuracy |
| 2025 | **Efficient Long CoT** | Prune redundant steps (overthinking problem) |

## The Overthinking Problem
Recent reasoning models generate excessive, redundant reasoning steps — even for simple questions like "1+1=?". This:
- Wastes compute at inference time
- Hurts distillation into smaller models
- Can actually *reduce* accuracy in some cases

See [[Overthinking in Reasoning Models]] for details.

## Key Papers
- [[Efficient Long CoT Reasoning in Small Language Models]]
- Wei et al. 2022 — Chain-of-Thought Prompting Elicits Reasoning
- Kojima et al. 2022 — Large Language Models are Zero-Shot Reasoners
- Wang et al. 2023 — Self-Consistency Improves CoT

## Related Concepts
- [[Test-time Compute Scaling]]
- [[Overthinking in Reasoning Models]]
- [[Knowledge Distillation]]
- [[Self-Consistency]]

## My Notes
CoT is the foundation of modern reasoning models. The recent shift from "make CoT longer" to "make CoT efficient" is an important pendulum swing — relevant for my SLM research where compute budget matters.
