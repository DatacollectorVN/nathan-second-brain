---
title: Mixture-of-Experts
tags: [concept, efficiency]
status: draft
related_papers:
  - "[[Dynamic Model Routing and Cascading for Efficient LLM Inference - A Survey]]"
date_added: 2026-06-28
---

## Definition
**Mixture-of-Experts (MoE)** is an architecture in which a gating/routing network activates only a sparse subset of internal "expert" sub-networks per token, increasing parameter count without a proportional increase in per-token compute.

## Intuition
A dense model runs every parameter for every token. MoE instead keeps many specialized expert blocks but, for each token, a small gate picks just a few to run. You get the capacity of a huge model at the FLOPs of a much smaller one.

## How It Works
A learned gate scores experts per token and routes the token to the top-k. Only those experts' weights participate in that token's forward pass. The routing here is **internal** — within one model's layers.

This is the key contrast drawn in [[Dynamic Model Routing and Cascading for Efficient LLM Inference - A Survey]]: MoE routes *within a single model*, whereas [[Model Routing]] and [[Model Cascading]] route *across multiple independently trained LLMs* at inference time. The survey explicitly scopes MoE out of its taxonomy for this reason.

## Variants & Evolution
> Stub — expand with Switch Transformer, GShard, sparse vs soft routing, load-balancing losses, and modern open MoE models when a dedicated paper is processed.

## Key Papers
- [[Dynamic Model Routing and Cascading for Efficient LLM Inference - A Survey]] (contrast only — MoE is out of scope there)

## Related Concepts
- [[Model Routing]]
- [[Model Cascading]]

## My Notes
Useful boundary marker: when someone says "routing," clarify intra-model (MoE) vs inter-model (this survey). Different cost models, different failure modes.
