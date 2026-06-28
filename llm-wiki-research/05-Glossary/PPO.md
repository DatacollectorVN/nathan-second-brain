---
title: PPO
tags: [glossary]
---

## Definition
**PPO (Proximal Policy Optimization)** — a policy-gradient reinforcement learning algorithm (Schulman et al., 2017) that improves a policy while clipping updates to keep each step close to the previous policy, trading a little optimality for much greater training stability.

## Context
A workhorse RL optimizer for LLMs, used both in [[RLHF]] and in RL-based routing. In [[Dynamic Model Routing and Cascading for Efficient LLM Inference - A Survey]], Router-R1 trains an LLM router with PPO on question–score pairs to make sequential think/route decisions. Compare [[GRPO]], a critic-free variant.

## See Also
- [[GRPO]]
- [[RLHF]]
- [[Contextual Bandit]]
