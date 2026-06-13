---
title: Supervised Fine-Tuning (SFT)
tags: [concept, training, fine-tuning]
status: stable
related_papers:
  - "[[Efficient Long CoT Reasoning in Small Language Models]]"
date_added: 2025-06-13
---

## Definition
Supervised Fine-Tuning (SFT) is the process of training a pre-trained language model on labeled (input, output) pairs to teach it specific behaviors, formats, or skills.

## Intuition
A pre-trained model is a generalist — it has read the internet but doesn't know how *you* want it to respond. SFT is how you "shape" it for a specific task: customer support, code generation, instruction following, or — in [[Efficient Long CoT Reasoning in Small Language Models]] — concise reasoning.

## How It Works
Given dataset `D = {(Q, Y)}` of question-answer pairs, minimize negative log-likelihood:

$$L_{SFT} = -\mathbb{E}_{(Q, Y) \sim D} \log M(Y | Q)$$

The model learns to maximize probability of `Y` given `Q`. Standard cross-entropy training.

## When to Use SFT
- **Instruction tuning** — teach model to follow instructions
- **Domain adaptation** — make model good at specific topics (medical, legal)
- **Format learning** — JSON outputs, specific structures
- **Distillation step** — first stage when learning from teacher outputs (see [[Knowledge Distillation]])
- **Pre-RLHF stage** — usually SFT comes before [[DPO]] or RLHF

## Typical Recipe
- 1-3 epochs (more risks overfitting)
- Low learning rate (1e-6 to 5e-5)
- Cosine schedule with warmup
- LoRA / QLoRA for parameter-efficient variants
- Pack multiple examples to maximize GPU utilization

## SFT in Pipeline
```
Pre-trained LM → SFT → DPO/RLHF → Deployed Model
   (general)    (task)   (preference)
```

## Limitations
- Only mimics the data — can't surpass demonstration quality
- Risk of catastrophic forgetting of pre-training knowledge
- No notion of "better" vs "worse" — every example treated as ground truth
- Cannot teach what to *avoid* (that's what [[DPO]] / RLHF is for)

## Key Papers
- Most modern instruction-tuning papers use SFT as a baseline
- [[Efficient Long CoT Reasoning in Small Language Models]] — uses SFT on pruned CoT data

## Related Concepts
- [[DPO]]
- [[RLHF]]
- [[Knowledge Distillation]]
- [[LoRA]]

## My Notes
SFT is the workhorse. Every fine-tuning project starts here. The interesting innovations are usually in *what data* you SFT on — like the on-policy pruned CoT data in the Efficient Long CoT paper.
