---
title: Exact Unlearning
tags: [concept, exact-unlearning]
status: draft
related_papers:
  - "[[A Survey of Machine Unlearning]]"
date_added: 2026-07-14
---

## Definition
> An unlearning process is exact (perfect) if the distribution of unlearned models is identical to the distribution of models retrained from scratch on the retained data.

## Intuition
No observer who only sees the resulting model can tell whether the forgotten data was ever used. Retraining trivially satisfies this, so retraining is sometimes called the only exact method — the game is achieving the same distributional guarantee cheaper, e.g. via sharded retraining ([[SISA]]).

## How It Works
General definition (per `01-Papers/A Survey of Machine Unlearning`, Def. 2):

$$Pr(A(D \setminus D_f) \in \mathcal{T}) = Pr(U(D, D_f, A(D)) \in \mathcal{T}) \quad \forall\, \mathcal{T} \subseteq \mathcal{H}$$

**What each symbol means:** $A$ = randomized learning algorithm; $U$ = unlearning algorithm; $D$ = training set; $D_f$ = forget set; $\mathcal{T}$ = any measurable subset of the hypothesis space $\mathcal{H}$; $Pr(\cdot)$ = distribution over models (randomized because of SGD batching, initialization, floating-point nondeterminism).
**Logic:** equality must hold for every event $\mathcal{T}$, i.e., the two model distributions are indistinguishable — the strongest possible unlearning guarantee. Interpreting $Pr(\cdot)$ over the weight space vs. the output space yields two notions; the output-space version is called *weak unlearning*.

## Variants & Evolution
- Weight-space vs. output-space (weak) exact unlearning.
- Streaming exact unlearning via $\rho$-TV-stability (Ullah et al.).
- Relaxations: [[Approximate Unlearning]] / [[Certified Removal]].

## Key Papers
- [[A Survey of Machine Unlearning]]

## Related Concepts
- [[Approximate Unlearning]]
- [[Certified Removal]]
- [[SISA]]

## My Notes
