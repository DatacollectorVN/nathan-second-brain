---
title: Model Routing
tags: [concept, efficiency, SLM]
status: draft
related_papers:
  - "[[Dynamic Model Routing and Cascading for Efficient LLM Inference - A Survey]]"
date_added: 2026-06-28
---

## Definition
**Model routing** is the inference-time selection of a single model from a pool of independently trained LLMs to answer a given query, based on the query's characteristics and each model's cost/capability profile.

## Intuition
Not every query needs your biggest, most expensive model. A trivial factual question can go to a cheap small model; a hard multi-step reasoning problem should go to a strong one. A *router* is a (usually lightweight) decision module that looks at the incoming query — and optionally model metadata like price, latency, and domain specialization — and dispatches it to the best-fit model. The router makes a single, one-shot decision per query (contrast with [[Model Cascading]], which decides sequentially).

## How It Works
Per the survey [[Dynamic Model Routing and Cascading for Efficient LLM Inference - A Survey]], routers can be characterized along three dimensions:

- **When** the decision is made: pre-generation (from the query alone) vs post-generation (after seeing a draft response) vs multi-stage.
- **What** signal is used: query features, model metadata, response-level signals, accumulated feedback.
- **How** it is computed: heuristic thresholds, supervised classifiers, bandit algorithms, or RL policies.

This is distinct from **[[Mixture-of-Experts]]**, which routes *within* a single model's layers; model routing routes *across* separate models.

## Variants & Evolution
The survey groups routers into six paradigms, each with its own decision mechanism and trade-offs. A recurring finding is that a well-designed router can beat the single best model in the pool by exploiting model complementarity. Each paradigm has its own concept note:

1. [[Difficulty-aware Routing]] — estimate query complexity, send easy→small / hard→large.
2. [[Preference-aligned Routing]] — train the router on human/synthetic preference data.
3. [[Clustering-based Routing]] — group queries by embedding, assign each cluster its best model.
4. [[Reinforcement Learning Routing]] — learn routing as a policy / bandit from reward feedback.
5. [[Uncertainty Quantification|Uncertainty-based Routing]] — route on model confidence.
6. [[Model Cascading|Cascading]] — cheap-first, verify, escalate sequentially.

## Key Papers
- [[Dynamic Model Routing and Cascading for Efficient LLM Inference - A Survey]]

## Related Concepts
- [[Model Cascading]]
- [[Mixture-of-Experts]]
- [[Uncertainty Quantification]]
- [[Small Language Models]]

## My Notes
The pre-/post-generation split maps cleanly onto my on-device interest: a pre-generation router keeps the SLM-only fast path cheap, while post-generation escalation is where cloud cost creeps back in. Worth tracking router overhead (latency/energy) as a first-class metric, not just routing accuracy.
