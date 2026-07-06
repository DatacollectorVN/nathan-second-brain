---
title: Differential Privacy
tags: [concept, privacy]
status: draft
related_papers:
  - A Comprehensive Guide to Differential Privacy
date_added: 2026-07-06
---

## Definition
> One-sentence definition.

Differential privacy (DP) is a mathematical framework guaranteeing that the output distribution of a randomized algorithm changes by at most a bounded (multiplicative) factor when any single individual's record is added to or removed from the input dataset.

## Intuition
> Explain it simply, as if to a colleague.

Anyone looking at the output cannot confidently tell whether a particular person's data was used, giving each individual plausible deniability regardless of what auxiliary information an adversary holds. Privacy is achieved "by process" — by injecting calibrated randomness into the computation.

## How It Works
> Technical detail, math if applicable.

For neighboring datasets $D\sim D'$ (differing in one record) and any output set $S$, a mechanism $\mathcal{A}$ is $\varepsilon$-DP if
$$P[\mathcal{A}(D)\in S] \le \exp(\varepsilon)\cdot P[\mathcal{A}(D')\in S].$$
The relaxation $(\varepsilon,\delta)$-DP adds a slack term $\delta$ (probability the guarantee fails), enabling Gaussian noise. Noise is calibrated to the [[Sensitivity]] of the query. Key properties: [[Composition]], [[Post-Processing Immunity]], and group privacy.

## Variants & Evolution
> How has this concept evolved across papers/models?

Pure $\varepsilon$-DP → [[Approximate Differential Privacy]] $(\varepsilon,\delta)$ → [[Renyi Differential Privacy]], zCDP, Gaussian DP (tighter composition), plus local / personalized / heterogeneous / sensitive-privacy variants for different trust models and utility trade-offs.

## Key Papers
- [[A Comprehensive Guide to Differential Privacy]]

## Related Concepts
- [[Approximate Differential Privacy]]
- [[Composition]]
- [[Sensitivity]]
- [[Membership Inference]]

## My Notes
DP is the parent framework for certified / approximate machine unlearning: the add-or-remove neighboring relation is exactly the "delete one record" operation, and $(\varepsilon,\delta)$-indistinguishability is reused to bound unlearned-vs-retrained models.
