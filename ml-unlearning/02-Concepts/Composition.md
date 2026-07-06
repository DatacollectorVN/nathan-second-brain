---
title: Composition
tags: [concept, privacy]
status: draft
related_papers:
  - A Comprehensive Guide to Differential Privacy
date_added: 2026-07-06
---

## Definition
> One-sentence definition.

Composition describes how the total privacy loss accumulates when multiple differentially private mechanisms are applied to the same (or partitioned) data.

## Intuition
> Explain it simply, as if to a colleague.

Every query spends part of a fixed [[Privacy Budget]]. Run more queries or demand more accuracy, and you spend more budget. Composition theorems tell you exactly how the spend adds up so you can stay within bounds.

## How It Works
> Technical detail, math if applicable.

**Sequential composition (Prop. 1).** Run $k$ mechanisms on the *same* data, each $(\varepsilon_i,\delta_i)$-DP:
$$\text{total} = \Big(\textstyle\sum_i\varepsilon_i,\ \sum_i\delta_i\Big)\text{-DP}.$$
*Logic:* each release leaks at most $\varepsilon_i$ worth of evidence about any one record; likelihood ratios multiply across independent releases, so their logs ($\varepsilon$'s) add. The failure probabilities $\delta_i$ add by a union bound. This is the pessimistic baseline — it assumes every query leaks its worst case simultaneously.

**Advanced composition (Thm. 1).** For $k$-fold *adaptive* composition (each mechanism may depend on previous outputs) of $(\varepsilon,\delta)$-DP mechanisms, for any $\delta'>0$:
$$\Big(\varepsilon',\ k\delta+\delta'\Big)\text{-DP},\qquad \varepsilon'=\sqrt{2k\ln(1/\delta')}\,\varepsilon+k\varepsilon(e^{\varepsilon}-1).$$

- $\sqrt{2k\ln(1/\delta')}\,\varepsilon$ — the dominant term. *Logic:* the realized privacy loss per step is a random variable, not always its worst case; over $k$ steps losses partially cancel like a random walk, so the typical total grows as $\sqrt{k}$, not $k$. This is a concentration (Azuma-type) bound.
- $k\varepsilon(e^{\varepsilon}-1)$ — the drift: each step's *expected* loss is slightly positive ($\approx\varepsilon^2$ for small $\varepsilon$, since $e^\varepsilon-1\approx\varepsilon$), and expectations do add linearly. Hence the small-$\varepsilon$ form $\varepsilon'\approx\sqrt{2k\ln(1/\delta')}\,\varepsilon+k\varepsilon^2$.
- $\delta'$ — the price of arguing "with high probability": you buy the $\sqrt{k}$ bound by tolerating an extra failure probability $\delta'$. Smaller $\delta'$ ⇒ larger $\ln(1/\delta')$ ⇒ larger $\varepsilon'$.
- *When it wins:* $\sqrt{k}$ beats $k$ once $k$ is large — many queries or training iterations. For small $k$ and moderate $\varepsilon$, plain summation can actually be tighter.

**Parallel composition (Prop. 2).** Mechanisms run on *disjoint* partitions of the data:
$$\text{total} = \big(\max_i\varepsilon_i,\ \max_i\delta_i\big)\text{-DP}.$$
*Logic:* any single individual's record lives in exactly one partition, so only one mechanism ever sees it — the others reveal nothing about that person. Loss does **not** compound. This is why histogram-style queries (and SISA-style sharding) are cheap in privacy terms.

**In practice.** Even advanced composition is too loose over thousands of iterations; iterative training uses [[Renyi Differential Privacy (RDP)]] / moments-accountant tracking (survey Example 17: $\varepsilon=1.26$ vs $9.34$ at 100 epochs), with GDP and PLD accountants as newer refinements.

## Variants & Evolution
> How has this concept evolved across papers/models?

Naive linear → advanced composition → moments accountant / RDP → GDP and PLD accounting, each giving tighter cumulative bounds for many-iteration settings like [[DP-SGD]].

## Key Papers
- [[A Comprehensive Guide to Differential Privacy]]

## Related Concepts
- [[Privacy Budget]]
- [[Renyi Differential Privacy (RDP)]]
- [[Post-Processing Immunity]]

## My Notes
Composition budgeting is the direct analogue of *deletion capacity* — how many unlearning requests a certified scheme can absorb before it must retrain.
