---
title: Contextual Bandit
tags: [concept, efficiency]
status: draft
related_papers:
  - "[[Dynamic Model Routing and Cascading for Efficient LLM Inference - A Survey]]"
date_added: 2026-06-28
---

## Definition
A **contextual bandit** is an online decision-making framework where, given a context (here, query features), an agent selects an action (which LLM to use) and observes a reward (correctness/cost), learning to balance exploration of options against exploitation of known-good ones.

## Intuition
It's the middle ground between a fixed rule and full reinforcement learning: each decision is one-shot (no long horizon), but you keep learning from feedback as queries arrive. For routing, the "arm" is a model, the "context" is the query, and the "reward" combines quality and cost — so the router gets better the longer it runs in production.

## How It Works
Per [[Dynamic Model Routing and Cascading for Efficient LLM Inference - A Survey]], bandit routers frame model selection as online learning. Common algorithms: **LinUCB** (linear upper-confidence-bound; used by PILOT and GreenServ), policy-gradient contextual bandits (MixLLM), Thompson sampling variants for dueling/preference feedback (FGTS.CDB), and time-increasing UCB (TI-UCB) for non-stationary, improving model pools. MetaLLM frames routing as a multi-armed bandit selecting the cheapest model likely to be correct.

## Variants & Evolution
Bandit methods adapt online but typically operate on query-level signals only; the survey flags that no current method pairs response-level signals with online adaptation, suggesting bandit-style escalation policies that refine thresholds from deployment feedback as future work.

## Key Papers
- [[Dynamic Model Routing and Cascading for Efficient LLM Inference - A Survey]]

## Related Concepts
- [[Model Routing]]
- [[PPO]]

## My Notes
Bandits are attractive operationally because they learn from cheap binary feedback (was the answer accepted?) without a labeled training set. The non-stationarity angle (TI-UCB) matters: model pools change as providers ship new versions.
