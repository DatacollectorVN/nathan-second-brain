---
title: Direct Preference Optimization (DPO)
tags: [concept, training, alignment, RLHF-alternative]
status: stable
related_papers:
  - "[[Efficient Long CoT Reasoning in Small Language Models]]"
date_added: 2025-06-13
---

## Definition
Direct Preference Optimization (DPO) is a fine-tuning method that teaches a language model to prefer "good" responses over "bad" ones directly from preference pairs, without training a separate reward model like in classical RLHF.

## Intuition
Classical RLHF needs three stages: SFT → train reward model → PPO. DPO collapses this into a single supervised step using a clever mathematical reformulation. You just give it pairs of (preferred, rejected) responses and it learns the preference directly.

## How It Works

### Setup
For each prompt Q, you provide:
- **Y (chosen)** — the preferred response
- **R (rejected)** — the less preferred response

### Loss Function
$$L_{DPO} = -\mathbb{E}\left[\log \sigma\left(\beta \log \frac{M(Y|Q)}{M_{ref}(Y|Q)} - \beta \log \frac{M(R|Q)}{M_{ref}(R|Q)}\right)\right]$$

Where:
- `M` is the model being trained
- `M_ref` is the frozen reference model (usually the SFT-trained model)
- `β = 0.1` (typically) controls how aggressively to deviate from reference
- `σ` is the logistic function

### Why It Works
The math shows that maximizing this loss is equivalent to RLHF's objective, but without needing an explicit reward model. The model learns to **increase** the relative probability of chosen responses and **decrease** that of rejected ones.

## Common Use Cases
- **Alignment** — teach model to refuse harmful prompts (chosen=refusal, rejected=compliance)
- **Style** — prefer concise over verbose (used in [[Efficient Long CoT Reasoning in Small Language Models]])
- **Quality** — prefer correct over incorrect reasoning
- **Safety** — reduce hallucinations

## Practical Tips
- Often combined with a small **SFT loss weight** (e.g., 0.3) to prevent likelihood collapse
- Reference model `M_ref` is typically the model after SFT, frozen during DPO
- Can over-fit fast — usually 1 epoch is enough
- Works best when chosen and rejected differ on a clear dimension (length, correctness, style)

## Variants
- **IPO** — Identity Preference Optimization (regularization variant)
- **KTO** — Kahneman-Tversky Optimization (single-response, not pairwise)
- **ORPO** — Odds Ratio Preference Optimization (removes need for ref model)
- **SimPO** — Simple Preference Optimization

## Key Papers
- Rafailov et al. 2023 — *Direct Preference Optimization: Your Language Model is Secretly a Reward Model* (original)
- [[Efficient Long CoT Reasoning in Small Language Models]] — uses DPO to prefer concise CoT over verbose

## Related Concepts
- [[RLHF]]
- [[SFT]]
- [[Knowledge Distillation]]

## My Notes
Used in the Efficient Long CoT paper to teach SLMs "concise reasoning is better than redundant reasoning" — chosen=pruned CoT, rejected=full bloated CoT. Elegant application: DPO is being repurposed beyond alignment into efficiency optimization.
