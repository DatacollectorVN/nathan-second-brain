---
title: ZRF Score
tags: [glossary, evaluation]
---

## Definition
> Zero Retrain Forgetting score: measures forgetting quality without needing a retrained reference model, by comparing the unlearned model's outputs on the forget set to a randomly initialized ("incompetent") teacher.

## Context
$$ZRF = 1 - \frac{1}{n_f}\sum_{i=0}^{n_f} JS\big(M(x_i), T_d(x_i)\big)$$

**What each symbol means:** $x_i$ = $i$-th forget-set sample; $n_f$ = forget-set size; $M$ = unlearned model; $T_d$ = randomly initialized teacher; $JS$ = Jensen–Shannon divergence between output distributions.
**Logic:** ZRF ≈ 1 means predictions on forgotten samples are indistinguishable from random (well forgotten); ZRF ≈ 0 means a systematic pattern remains. Introduced by Chundawat et al.; catalogued in [[A Survey of Machine Unlearning]].

## See Also
- [[Anamnesis Index (AIN)]]
- [[A Survey of Machine Unlearning]]
