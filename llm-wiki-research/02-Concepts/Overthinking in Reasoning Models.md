---
title: Overthinking in Reasoning Models
tags: [concept, reasoning, efficiency, problem]
status: stable
related_papers:
  - "[[Efficient Long CoT Reasoning in Small Language Models]]"
date_added: 2025-06-13
---

## Definition
Overthinking is a failure mode of large reasoning models (DeepSeek-R1, o1, QwQ) where they generate excessive, redundant, or circular reasoning steps — even for simple questions — inflating output length without improving accuracy and sometimes *hurting* it.

## Intuition
Imagine asking someone "what's 1+1?" and they reply with five paragraphs second-guessing whether you mean integer addition, whether there's hidden context, whether it's a trick question, before finally saying "2". That's overthinking. Modern reasoning models trained with RL on "think more = better" objectives have learned this pathology.

## Concrete Example (from the paper)
**Question:** "What is 1+1?"

**DeepSeek-R1 response:** ~300+ tokens of reasoning including:
- "But wait, could there be a different context..."
- "Is there any scenario where 1+1 doesn't equal 2?"
- "Another angle: could this be a trick question involving units?"
- "Maybe the user is testing if I can recognize..."
- Eventually: "The answer should be 2"

The actual answer "2" requires ~82 tokens of legitimate reasoning. The remaining ~250+ tokens are overthinking.

## Why It Happens
1. **RL reward design** — if you reward correctness without penalizing length, longer CoT becomes correlated with correctness during training
2. **Distribution shift** — model learned long reasoning is "safe" for hard problems, applies it to easy ones too
3. **Self-verification habit** — models trained to "double-check" do so even when unnecessary
4. **No early-stopping signal** — model doesn't know when it has "enough" reasoning

## Why It's Bad
- **Inference cost** — pay for tokens that don't help
- **Latency** — slower responses
- **Accuracy degradation** — over-reasoning can introduce errors (Marjanović et al., 2025)
- **Distillation harm** — overthinking traces hurt SLM training (the key finding of [[Efficient Long CoT Reasoning in Small Language Models]])

## Mitigation Strategies

| Approach | Method | Pro / Con |
|----------|--------|-----------|
| **Length-based RL rewards** | Penalize long CoT during RL | Computationally expensive |
| **Heuristic truncation (FCS)** | Keep only minimal correct prefix | Ignores model-specific needs |
| **Prompting tricks** | "Be concise" or token budgets | Brittle, doesn't change weights |
| **Binary cutting + on-policy** | Smart pruning + SLM-aware validation | Proposed by [[Efficient Long CoT Reasoning in Small Language Models]] |

## Empirical Stats
From the Efficient Long CoT paper:
- ~40% of long CoT samples can be **pruned by >50%** without accuracy loss
- 61.83% of samples have the correct answer in the **last 10 steps** (not the early ones)
- Llama-3.1-8B distilled on full CoT generated **1051 tokens** on GSM8K vs **339 tokens** with their efficient method — same accuracy

## Key Papers
- Chen et al. 2025 — *Do Not Think That Much for 2+3=?: On the Overthinking of o1-like LLMs*
- Sui et al. 2025 — *Stop Overthinking: A Survey on Efficient Reasoning*
- Marjanović et al. 2025 — *DeepSeek-R1 Thoughtology*
- [[Efficient Long CoT Reasoning in Small Language Models]]

## Related Concepts
- [[Chain-of-Thought]]
- [[Test-time Compute Scaling]]
- [[Knowledge Distillation]]

## My Notes
This is an important counter-narrative to "scale CoT length". For my SLM research, efficiency matters as much as capability — overthinking is the enemy of on-device deployment. The paradox: we want SLMs to reason like big models without inheriting their inefficiencies.
