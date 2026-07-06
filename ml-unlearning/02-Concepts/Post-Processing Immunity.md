---
title: Post-Processing Immunity
tags: [concept, privacy]
status: draft
related_papers:
  - A Comprehensive Guide to Differential Privacy
date_added: 2026-07-06
---

## Definition
> One-sentence definition.

Post-processing immunity is the property that applying any (possibly randomized) function to the output of a differentially private mechanism cannot weaken its privacy guarantee.

## Intuition
> Explain it simply, as if to a colleague.

Once data has been protected by DP, no downstream analyst or adversary can "undo" the protection by crunching the released numbers further — as long as they don't touch the raw data again.

## How It Works
> Technical detail, math if applicable.

If $\mathcal{A}$ is $(\varepsilon,\delta)$-DP, then for any function $g:\mathcal{R}\to\mathcal{R}'$, the composition $g\circ\mathcal{A}$ defined by $(g\circ\mathcal{A})(D)=g(\mathcal{A}(D))$ is also $(\varepsilon,\delta)$-DP (Prop. 3, Dwork & Roth 2014).

## Variants & Evolution
> How has this concept evolved across papers/models?

A foundational, invariant property — holds across all DP variants and is a key reason DP is considered "future-proof."

## Key Papers
- [[A Comprehensive Guide to Differential Privacy]]

## Related Concepts
- [[Differential Privacy]]
- [[Composition]]

## My Notes
Post-processing is why a DP-trained (or DP-unlearned) model can be freely deployed, queried, and fine-tuned without extra privacy loss.
