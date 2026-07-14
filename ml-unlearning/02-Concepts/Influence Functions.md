---
title: Influence Functions
tags: [concept, certified]
status: draft
related_papers:
  - "[[Understanding Black-box Predictions via Influence Functions]]"
date_added: 2026-07-13
---

## Definition
> An influence function measures how a model's parameters (or any function of them, like a test loss) change when a training point is upweighted by an infinitesimal amount — giving a closed-form, retraining-free approximation of leave-one-out effects.

## Intuition
Instead of deleting a point and retraining to see what it contributed, take the derivative of "the trained model" with respect to that point's weight. Removing a point is just upweighting it by $\epsilon = -\frac{1}{n}$, so one derivative approximates the whole retrain. Curvature matters: a point pulling in a direction the rest of the data doesn't constrain has outsized influence.

## How It Works
For an empirical risk minimizer $\hat\theta$ with positive-definite Hessian $H_{\hat\theta} = \frac{1}{n}\sum_i \nabla^2_\theta L(z_i,\hat\theta)$, the influence of upweighting training point $z$ on the parameters is

$$\mathcal{I}_{\text{up,params}}(z) = -H_{\hat\theta}^{-1}\,\nabla_\theta L(z,\hat\theta)$$

**What each symbol means:** $\nabla_\theta L(z,\hat\theta)$ is the loss gradient at $z$; $H_{\hat\theta}^{-1}$ rescales it by inverse curvature of the total risk.
**Logic:** it is a single Newton step on the quadratic approximation of the risk around $\hat\theta$; the approximate effect of deleting $z$ is $\hat\theta_{-z} - \hat\theta \approx -\frac{1}{n}\mathcal{I}_{\text{up,params}}(z)$. Chain-ruling through $\hat\theta$ gives the influence on a test loss: $\mathcal{I}_{\text{up,loss}}(z, z_{\text{test}}) = -\nabla_\theta L(z_{\text{test}},\hat\theta)^\top H_{\hat\theta}^{-1} \nabla_\theta L(z,\hat\theta)$. Computed at scale via [[Hessian-Vector Product (HVP)]] tricks (CG or stochastic Taylor-series estimation) rather than explicit inversion.

## Variants & Evolution
- Robust statistics origin: Hampel (1974), infinitesimal jackknife (Jaeckel, 1972), Cook & Weisberg (1982) for linear-model case deletion.
- Koh & Liang (2017) scale it to modern ML and relax convexity/differentiability empirically (damping, smooth surrogates).
- Guo et al. (2020, [[Certified Data Removal from Machine Learning Models]]) turn the influence Newton step into a *certified* deletion mechanism: [[Strong Convexity]] bounds the residual, loss perturbation masks it — the concrete realization of [[Certified Removal]].
- Later work questions fragility in large deep networks (to be captured when those papers are processed).

## Key Papers
- [[Understanding Black-box Predictions via Influence Functions]]
- [[Certified Data Removal from Machine Learning Models]] (Newton deletion = influence step + certificate)

## Related Concepts
- [[Hessian-Vector Product (HVP)]]
- [[Certified Removal]]
- [[Data Poisoning]]
- [[Membership Inference]]

## My Notes
Stub created by Worker while processing Koh & Liang 2017 — expand with deletion-capacity and fragility discussion once related papers are in.
