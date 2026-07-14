---
title: Anamnesis Index (AIN)
tags: [glossary, evaluation]
---

## Definition
> A relearn-time-based unlearning metric: the ratio of steps the unlearned model needs to recover accuracy on forgotten classes versus a from-scratch model.

## Context
$$AIN = \frac{r_t(M_u, M_{orig}, \alpha)}{r_t(M_s, M_{orig}, \alpha)}$$

**What each symbol means:** $r_t(M, M_{orig}, \alpha)$ = relearn mini-batches for model $M$ to reach within $\alpha\%$ of the original model $M_{orig}$'s accuracy on the forgotten classes; $M_u$ = unlearned model; $M_s$ = model trained from scratch.
**Logic:** AIN ≈ 1 = genuine forgetting (relearning is as hard as learning fresh); AIN ≪ 1 = knowledge retained; AIN ≫ 1 = parameters pushed so far the unlearning is detectable (Streisand effect). Used for zero-shot unlearning evaluation; catalogued in [[A Survey of Machine Unlearning]].

## See Also
- [[ZRF Score]]
- [[A Survey of Machine Unlearning]]
