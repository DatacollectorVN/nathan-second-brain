---
title: Laplace Mechanism
family: Basic DP Mechanisms
tags: [architecture, privacy]
status: draft
year: 2006
params:
context_length:
date_added: 2026-07-06
---

## Overview
> What is this architecture and why does it matter?

The Laplace mechanism is one of the foundational DP tools: it adds Laplace-distributed noise, calibrated to the $\ell_1$-sensitivity of a numeric query, to achieve pure $\varepsilon$-DP. It is the canonical mechanism for releasing aggregated statistics like counts, sums, and means.

## Key Design Choices
- Adds i.i.d. noise $Y_i \sim \text{Lap}(\Delta^1_f/\varepsilon)$ to each of the $d$ output dimensions.
- Noise scale grows with $\ell_1$-[[Sensitivity]] and shrinks as $\varepsilon$ grows.
- Achieves pure $\varepsilon$-DP (no $\delta$ slack).

## Comparison to Previous
| Feature | Laplace Mechanism | Gaussian Mechanism |
|---------|-------------------|--------------------|
| Guarantee | Pure $\varepsilon$-DP | $(\varepsilon,\delta)$-DP |
| Sensitivity norm | $\ell_1$ | $\ell_2$ |
| Best for | Simple aggregate statistics | High-composition ML |

## Training Details
- Dataset: any numeric-output query.
- Compute: negligible.
- Techniques: Laplace pdf $g(u)=\frac{1}{2b}\exp(-|u|/b)$, variance $2b^2$; theorem: $\mathcal{A}_L(D;f,\varepsilon)=f(D)+(Y_1,\dots,Y_d)$ is $\varepsilon$-DP.

## Strengths & Weaknesses
**Strengths:** simple, pure $\varepsilon$-DP, widely used for statistics.
**Weaknesses:** looser composition than Gaussian/RDP for iterative ML; $\ell_1$ calibration can be noisier in high dimensions.

## Key Papers
- [[A Comprehensive Guide to Differential Privacy]]

## Related
- [[Gaussian Mechanism]]
- [[Sensitivity]]
