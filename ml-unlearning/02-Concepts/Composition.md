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

- **Sequential (Prop. 1):** $k$ mechanisms, each $(\varepsilon_i,\delta_i)$-DP → $\left(\sum_i\varepsilon_i,\sum_i\delta_i\right)$-DP.
- **Advanced (Thm. 1):** adaptive composition of $(\varepsilon,\delta)$-DP mechanisms → $(\varepsilon',k\delta+\delta')$-DP with $\varepsilon'=\sqrt{2k\ln(1/\delta')}\,\varepsilon+k\varepsilon(e^{\varepsilon}-1)$.
- **Parallel (Prop. 2):** on disjoint partitions → $(\max_i\varepsilon_i,\max_i\delta_i)$-DP; loss does *not* compound.

Tighter accounting via [[Renyi Differential Privacy]] / moments accountant is used in practice for iterative training.

## Variants & Evolution
> How has this concept evolved across papers/models?

Naive linear → advanced composition → moments accountant / RDP → GDP and PLD accounting, each giving tighter cumulative bounds for many-iteration settings like [[DP-SGD]].

## Key Papers
- [[A Comprehensive Guide to Differential Privacy]]

## Related Concepts
- [[Privacy Budget]]
- [[Renyi Differential Privacy]]
- [[Post-Processing Immunity]]

## My Notes
Composition budgeting is the direct analogue of *deletion capacity* — how many unlearning requests a certified scheme can absorb before it must retrain.
