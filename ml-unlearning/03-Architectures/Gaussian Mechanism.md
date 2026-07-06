---
title: Gaussian Mechanism
family: Basic DP Mechanisms
tags: [architecture, privacy]
status: draft
year: 2014
params:
context_length:
date_added: 2026-07-06
---

## Overview
> What is this architecture and why does it matter?

The Gaussian mechanism adds Gaussian noise calibrated to the $\ell_2$-sensitivity of a numeric query, providing $(\varepsilon,\delta)$-DP (approximate DP). It is the mechanism of choice in high-composition / ML settings — including [[DP-SGD]] — because it composes well under [[Renyi Differential Privacy]].

## Key Design Choices
- Calibrates noise to $\ell_2$-sensitivity (vs. $\ell_1$ for the [[Laplace Mechanism]]).
- Provides $(\varepsilon,\delta)$-DP with a small failure probability $\delta$, not pure $\varepsilon$-DP.
- Composition theorems make cumulative loss tractable for iterative algorithms.

## Comparison to Previous
| Feature | Gaussian Mechanism | Laplace Mechanism |
|---------|--------------------|-------------------|
| Noise distribution | Gaussian $\mathcal{N}(0,\sigma^2)$ | Laplace $\text{Lap}(b)$ |
| Sensitivity norm | $\ell_2$ | $\ell_1$ |
| Guarantee | $(\varepsilon,\delta)$-DP | Pure $\varepsilon$-DP |

## Training Details
- Dataset: any numeric-output query.
- Compute: negligible.
- Techniques: pairs with moments-accountant style privacy tracking.

## Strengths & Weaknesses
**Strengths:** excellent composition, well-suited to high-dimensional gradients and repeated analyses.
**Weaknesses:** only approximate DP (allows $\delta$ failure), needs $\ell_2$-sensitivity bounds.

## Key Papers
- [[A Comprehensive Guide to Differential Privacy]]

## Related
- [[Laplace Mechanism]]
- [[DP-SGD]]
- [[Sensitivity]]
