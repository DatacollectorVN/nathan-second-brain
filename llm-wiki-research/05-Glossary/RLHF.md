---
title: RLHF (Reinforcement Learning from Human Feedback)
tags: [glossary, alignment]
---

## Definition
A fine-tuning method that aligns a language model with human preferences: humans rank model outputs, a reward model learns those rankings, and the LM is optimized (usually via PPO, with a KL penalty) to maximize that reward.

## Context
The classic 3-stage alignment pipeline — SFT → reward model → PPO — popularized by InstructGPT (Ouyang et al., 2022). Contrast with [[DPO]], which achieves a similar objective without a separate reward model or RL loop. Reasoning models like [[DeepSeek-R1]] use a variant (GRPO) with verifiable rewards.

## See Also
- [[RLHF]] (full concept note in 02-Concepts)
- [[DPO]]
- [[SFT]]
