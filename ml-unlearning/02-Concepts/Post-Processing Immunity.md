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

If $\mathcal{A}$ is $(\varepsilon,\delta)$-DP, then for **any** function $g:\mathcal{R}\to\mathcal{R}'$, the composition
$$(g\circ\mathcal{A})(D)=g(\mathcal{A}(D))$$
is also $(\varepsilon,\delta)$-DP (Prop. 3, Dwork & Roth 2014).

**What each symbol means**

- $\mathcal{A}:\mathcal{D}\to\mathcal{R}$ — the DP mechanism; its output space $\mathcal{R}$ could be noisy statistics or trained model weights.
- $g$ — *any* downstream computation: rounding, plotting, aggregation, model fine-tuning, or a deliberate attack. $g$ may be randomized, adversarial, and computationally unbounded — but it sees only $\mathcal{A}$'s output, **not** the raw data $D$.
- $g\circ\mathcal{A}$ — the end-to-end pipeline "privatize, then process," which inherits the same $(\varepsilon,\delta)$ unchanged.

**The logic.** The DP guarantee says the *distributions* of $\mathcal{A}(D)$ and $\mathcal{A}(D')$ are within $(e^\varepsilon,\delta)$ of each other. Applying the same map $g$ to two nearly-indistinguishable distributions cannot pull them further apart — this is the data-processing inequality: distinguishability can only shrink or stay equal under a shared transformation. Intuitively, $g$ has no new information about $D$ to inject; anything it "reveals" was already computable from the protected output.

**Boundary condition.** Immunity holds only while $g$ never touches the raw data again. A pipeline that re-reads $D$ (or another DP release of $D$) is not post-processing — that's *composition*, and the budgets add per [[Composition]].

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
