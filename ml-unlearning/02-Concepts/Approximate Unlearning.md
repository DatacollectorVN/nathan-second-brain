---
title: Approximate Unlearning
tags: [concept, certified]
status: draft
related_papers:
  - "[[A Survey of Machine Unlearning]]"
  - "[[Certified Data Removal from Machine Learning Models]]"
date_added: 2026-07-14
---

## Definition
> Unlearning that only requires the unlearned model's distribution to be *close* (within multiplicative $e^{\epsilon}$ and optionally additive $\delta$) to the retrained model's distribution, instead of identical.

## Intuition
Exact distributional equality is expensive; approximate unlearning trades a bounded, quantifiable amount of residual influence for large compute savings — the same bargain [[Approximate Differential Privacy]] makes relative to pure DP.

## How It Works
$\epsilon$-approximate unlearning (per `01-Papers/A Survey of Machine Unlearning`, §3.3):

$$e^{-\epsilon} \le \frac{Pr(U(D, z, A(D)) \in \mathcal{T})}{Pr(A(D \setminus z) \in \mathcal{T})} \le e^{\epsilon}$$

**What each symbol means:** $z$ = removed sample; $U(D, z, A(D))$ = unlearned model; $A(D \setminus z)$ = retrained model; $\mathcal{T} \subseteq \mathcal{H}$ = any event over models; $\epsilon > 0$ = indistinguishability budget.
**Logic:** the likelihood ratio between "unlearned" and "retrained" is bounded on a log scale by $\epsilon$, so no test can distinguish the two much better than the budget allows.

$(\epsilon,\delta)$ relaxation:

$$Pr(U(D, z, A(D)) \in \mathcal{T}) \le e^{\epsilon} Pr(A(D \setminus z) \in \mathcal{T}) + \delta$$

(and symmetrically). **What each symbol means:** $\delta > 0$ upper-bounds the probability that the pure $\epsilon$ bound fails. **Logic:** with probability $\ge 1-\delta$ the multiplicative guarantee holds; both directions are needed so neither distribution dominates.

## Variants & Evolution
- [[Certified Removal]] (Guo et al.): Newton-step deletion + loss perturbation for convex models.
- Descent-to-Delete (Neel et al.): perturbed gradient descent, weakly convex losses.
- Fisher/scrubbing approaches (Golatkar et al.) for DNNs, without full guarantees.

## Key Papers
- [[A Survey of Machine Unlearning]]
- [[Certified Data Removal from Machine Learning Models]]

## Related Concepts
- [[Exact Unlearning]]
- [[Certified Removal]]
- [[Differential Privacy]]
- [[Approximate Differential Privacy]]

## My Notes
