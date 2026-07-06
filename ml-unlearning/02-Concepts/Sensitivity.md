---
title: Sensitivity
tags: [concept, privacy]
status: draft
related_papers:
  - A Comprehensive Guide to Differential Privacy
date_added: 2026-07-06
---

## Definition
> One-sentence definition.

The (global $\ell_p$-) sensitivity of a query is the maximum change in its output when a single record is added, removed, or modified — it determines how much noise DP must add.

## Intuition
> Explain it simply, as if to a colleague.

If one person can swing the answer a lot, you must add a lot of noise to hide them; if their effect is tiny, little noise suffices. Sensitivity is the "how much can one person matter" knob.

## How It Works
> Technical detail, math if applicable.

For $f:\mathcal{D}\to\mathbb{R}^d$ and all neighbor pairs $N$:
$$\Delta^p_f=\max_{(D,D')\in N}\lVert f(D)-f(D')\rVert_p.$$
A counting query has $\Delta^1_f=1$. Noise scale is proportional to $\Delta^p_f$. Unbounded sensitivity (e.g., an average with no cap) is handled by **clipping**, which trades bias for bounded sensitivity — the same clipping used per-gradient in [[DP-SGD]].

## Variants & Evolution
> How has this concept evolved across papers/models?

$\ell_1$-sensitivity pairs with the [[Laplace Mechanism]]; $\ell_2$-sensitivity pairs with the [[Gaussian Mechanism]]. Local/smooth sensitivity variants exist for data-dependent noise calibration.

## Key Papers
- [[A Comprehensive Guide to Differential Privacy]]

## Related Concepts
- [[Differential Privacy]]
- [[Laplace Mechanism]]
- [[Gaussian Mechanism]]

## My Notes
Gradient clipping in DP-SGD is literally a sensitivity-bounding step — the same idea recurs in certified-unlearning influence bounds.
