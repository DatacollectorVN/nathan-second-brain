---
title: Certified Removal
tags: [concept, certified]
status: needs-review
related_papers:
  - "[[Certified Data Removal from Machine Learning Models]]"
  - "[[Understanding Black-box Predictions via Influence Functions]]"
date_added: 2026-07-13
---

## Definition
> Certified removal is approximate unlearning with a formal guarantee: after a deletion update, the model's distribution is provably (ε,δ)-indistinguishable from one retrained from scratch without the deleted point.

## Intuition
Take an influence-function-style Newton step to erase a point's contribution, then rely on calibrated randomness so any residual trace is statistically masked — a differential-privacy-flavored guarantee applied to deletion rather than training.

## How It Works
Grounded in [[Certified Data Removal from Machine Learning Models]] (Guo et al., ICML 2020). The deletion update is the removal approximation from [[Influence Functions]] — one Newton step:

$$w^- = w^* + H_{w^*}^{-1}\Delta, \qquad \Delta = \lambda w^* + \nabla\ell\left((w^*)^\top x_n,\, y_n\right)$$

**What each symbol means:** $w^*$ = trained parameters; $H_{w^*}$ = Hessian of the regularized loss on the remaining data at $w^*$; $\Delta$ = total gradient contribution of the deleted point $(x_n, y_n)$; $\lambda$ = $L_2$ regularization constant.
**Logic:** the Newton step cancels the deleted point's first-order contribution. [[Strong Convexity]] (manufactured by the regularizer) bounds the leftover gradient residual by $\frac{4\gamma C^2}{\lambda^2(n-1)}$ (Guo et al., Thm 1), and a loss-perturbation term $b^\top w$ added at training masks that residual — Gaussian $b$ yields $(\epsilon,\delta)$-CR with $\delta = 1.5\,e^{-c^2/2}$, mirroring [[Differential Privacy]].

## Variants & Evolution
- **ε-CR vs (ε,δ)-CR:** pure certification with Laplace-like perturbation noise ($p(b) \propto e^{-\frac{\epsilon}{\epsilon'}\lVert b\rVert_2}$) vs. the Gaussian relaxation with failure probability δ — directly parallel to pure vs. [[Approximate Differential Privacy]].
- **Exact special case:** for least-squares regression the Newton step is exact (loss is quadratic), giving ε-CR with ε = 0 and no noise needed.
- **Newton step + loss perturbation** (Guo et al. 2020): the canonical mechanism for $L_2$-regularized differentiable convex (e.g. logistic) losses.
- **Batch removal + β-budget accounting** (Algorithm 2): per-removal data-dependent residual bounds accumulate against a budget $\sigma\epsilon/c$; batch error grows as $m^2$; retraining triggers when the budget is spent — see [[Deletion Capacity]].
- **Non-linear extension via feature extractors:** CR linear head on a *public* pretrained extractor, or on a [[DP-SGD]]-trained extractor with additive composition of guarantees $(\epsilon_{DP}+\epsilon_{CR},\, \delta_{DP}+\delta_{CR})$.
- **Beyond convexity:** open frontier — nonconvex certificates require different tools (e.g. rewind-based methods; to be captured when those papers are processed).

## Key Papers
- [[Certified Data Removal from Machine Learning Models]] (Guo et al. 2020 — the defining paper)
- [[Understanding Black-box Predictions via Influence Functions]] (foundation)

## Related Concepts
- [[Influence Functions]]
- [[Strong Convexity]]
- [[Deletion Capacity]]
- [[Differential Privacy]]
- [[(epsilon-delta)-DP]]

## My Notes
Upgraded from stub after processing Guo et al. 2020 (Worker pass applying Reviewer fixes, 2026-07-13). Next: contrast with nonconvex certificates (Rewind-to-Delete, noisy-SGD unlearning) in a `06-Critical-Reviews/` note.
