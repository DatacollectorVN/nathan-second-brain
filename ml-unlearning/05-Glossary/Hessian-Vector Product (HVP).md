---
title: Hessian-Vector Product (HVP)
tags: [glossary]
---

## Definition
> The product $Hv = [\nabla^2_\theta L(\theta)]\,v$ of a loss Hessian with an arbitrary vector, computable implicitly (Pearlmutter, 1994) in the same $O(p)$ time as one gradient — without ever forming the $p \times p$ Hessian. Here $p$ is the parameter count and $v$ any vector in $\mathbb{R}^p$; the trick differentiates the directional derivative $\nabla_\theta\left(\nabla_\theta L^\top v\right)$.

## Context
The workhorse for scaling second-order quantities to deep nets: influence functions, Newton-step unlearning updates, and CG/stochastic solvers for $H^{-1}v$ all reduce to sequences of HVPs. Supported natively by autodiff frameworks.

## See Also
- [[Influence Functions]]
- [[Understanding Black-box Predictions via Influence Functions]]
