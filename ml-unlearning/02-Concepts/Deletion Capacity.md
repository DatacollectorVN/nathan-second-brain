---
title: Deletion Capacity
tags: [concept, certified]
status: draft
related_papers:
  - "[[Certified Data Removal from Machine Learning Models]]"
date_added: 2026-07-13
---

## Definition
> Deletion capacity is the number of removal requests an approximate-unlearning scheme can honor before its accumulated error exhausts the certification budget and full retraining becomes mandatory.

## Intuition
The unlearning analogue of a [[Privacy Budget]]: every approximate deletion leaves a small residual error, and the certificate only holds while the total residual stays under what the injected noise can mask. Spend the budget, retrain from scratch.

## How It Works
Concrete instantiation in [[Certified Data Removal from Machine Learning Models]] (Algorithm 2): each Newton-step removal contributes its data-dependent gradient-residual bound (Corollaries 1/2) to an accumulator

$$\beta \leftarrow \beta + \gamma\,\lVert X_-\rVert_2\,\lVert H_{w^*}^{-1}\Delta\rVert_2\,\lVert X_- H_{w^*}^{-1}\Delta\rVert_2, \qquad \text{retrain when } \beta > \sigma\epsilon/c$$

**What each symbol means:** $\beta$ = cumulative gradient-residual norm over all removals so far; $\sigma$ = std. dev. of the loss-perturbation vector $b$; $\epsilon$ = certification parameter; $c$ = the Gaussian constant setting $\delta = 1.5e^{-c^2/2}$; the added term is the per-removal residual bound.
**Logic:** the perturbation $b$ can only mask so much total residual at a given $(\epsilon,\delta)$ — the budget $\sigma\epsilon/c$ is that masking capacity, so capacity trades off directly against accuracy (larger $\sigma$ hurts utility but buys more deletions). Empirically (LSUN), accepting 88.6% → 83.3% accuracy bought >10,000 removals.

## Variants & Evolution
- Mirrors sequential [[Composition]] in [[Differential Privacy]]: deletions compose like queries against a privacy budget.
- Batch removal spends budget quadratically in batch size $m$ (single-Hessian approximation) vs. linearly one-by-one.
- Later work formalizes deletion capacity as a quantity in its own right (to be captured when those papers are processed).

## Key Papers
- [[Certified Data Removal from Machine Learning Models]]

## Related Concepts
- [[Privacy Budget]]
- [[Composition]]
- [[Certified Removal]]
- [[Influence Functions]]

## My Notes
Stub created by Worker applying Reviewer fixes to the Guo et al. 2020 note.
