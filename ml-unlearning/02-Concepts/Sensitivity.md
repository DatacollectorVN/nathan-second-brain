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

For a query $f:\mathcal{D}\to\mathbb{R}^d$ and the set $N$ of all neighboring dataset pairs:
$$\Delta^p_f=\max_{(D,D')\in N}\lVert f(D)-f(D')\rVert_p.$$

**What each symbol means**

- $f$ — the query being privatized: a count, mean, histogram, or an entire training procedure's gradient. Its output is a $d$-dimensional vector.
- $N$ — every possible pair of datasets differing in one record. The max is over *all* such pairs (hence **global** sensitivity), not just the dataset at hand — the bound must hold no matter what the data turns out to be, and computing it must not itself depend on private data.
- $\lVert\cdot\rVert_p$ — how we measure the output shift. $p=1$ (sum of absolute coordinate changes) pairs with Laplace noise; $p=2$ (Euclidean length) pairs with Gaussian noise. The norm must match the mechanism because each mechanism's DP proof bounds the likelihood ratio in terms of that specific norm.
- $\Delta^p_f$ — the worst-case influence any single person can have on the answer.

**The logic.** DP hides *one person's contribution* inside noise. The noise therefore has to be at least as large as the biggest dent one person can make: Laplace uses scale $b=\Delta^1_f/\varepsilon$, the Gaussian mechanism uses $\sigma\propto\Delta^2_f\sqrt{2\ln(1.25/\delta)}/\varepsilon$. Two consequences:

- *Low-sensitivity queries are cheap.* A count changes by at most 1 when one record changes ($\Delta^1_f=1$); a mean over $n$ records moves by at most $\text{range}/n$ — so aggregates over large datasets need almost no noise.
- *High-sensitivity queries are expensive.* A max or an unbounded sum can move arbitrarily far ⇒ infinite sensitivity ⇒ no finite noise suffices.

**Clipping.** The fix for unbounded sensitivity is to *impose* a bound: cap each individual's contribution at $C$ (clip gradients, truncate values). Then $\Delta_f \le C$ by construction and noise can be calibrated to $C$. The cost is **bias** — large true contributions are distorted — giving a bias–variance trade-off in choosing $C$: too small ⇒ heavy clipping bias; too large ⇒ heavy noise. This is exactly the per-example gradient clipping step in [[DP-SGD]].

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
