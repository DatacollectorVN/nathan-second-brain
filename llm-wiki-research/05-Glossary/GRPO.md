---
title: GRPO
tags: [glossary]
---

## Definition
**GRPO (Group Relative Policy Optimization)** — a reinforcement learning algorithm (Shao et al., 2024) that replaces PPO's learned value/critic with a baseline computed from the relative rewards of a group of sampled outputs, reducing memory and compute.

## Context
Popularized by DeepSeek for reasoning training and reused in routing. In [[Dynamic Model Routing and Cascading for Efficient LLM Inference - A Survey]], R2-Reasoner uses GRPO in a staged SFT+RL pipeline to train its task decomposer and subtask allocator, and SCOPE uses GRPO to train a performance estimator (reward-shaped prediction) rather than a routing policy. Compare [[PPO]].

## See Also
- [[PPO]]
- [[RLHF]]
