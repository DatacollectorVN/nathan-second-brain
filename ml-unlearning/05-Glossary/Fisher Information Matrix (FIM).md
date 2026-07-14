---
title: Fisher Information Matrix (FIM)
tags: [glossary]
---

## Definition
> A matrix quantifying how much information the model parameters carry about the data — the covariance of the score (gradient of the log-likelihood). Widely used in unlearning as a cheap, positive-semi-definite approximation of the Hessian.

## Context
Empirical approximation (per [[A Survey of Machine Unlearning]], Eq. 22):

$$\mathcal{I}(w; D) \approx \frac{1}{|D|}\sum_{x,y \in D}\left(\frac{\partial \log p(y|x;w)}{\partial w}\right)^2$$

**What each symbol means:** $w$ = model parameters; $D$ = dataset; $p(y|x;w)$ = model's predictive likelihood.
**Logic:** large entries mean small parameter changes strongly affect the data likelihood, i.e., the parameters "know" a lot about the data. Fisher-based unlearning (Golatkar et al., Peste et al.) uses the FIM in place of the expensive Hessian in [[Influence Functions]]-style updates, plus noise injection to mask residuals; the FIM trace also underlies the epistemic-uncertainty *efficacy* metric.

## See Also
- [[Influence Functions]]
- [[Hessian-Vector Product (HVP)]]
- [[A Survey of Machine Unlearning]]
