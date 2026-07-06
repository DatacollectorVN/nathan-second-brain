---
title: DP-SGD
family: Gradient Perturbation
tags: [architecture, privacy]
status: draft
year: 2016
params:
context_length:
date_added: 2026-07-06
---

## Overview
> What is this architecture and why does it matter?

Differentially Private Stochastic Gradient Descent (DP-SGD) is the workhorse method for training deep models with $(\varepsilon,\delta)$-DP. Outlined for simple models by Song et al. (2013) and made practical for deep nets by Abadi et al. (2016) together with the moments accountant. Implemented in TensorFlow Privacy, Opacus (PyTorch), and Diffprivlib.

## Key Design Choices
- **Per-example gradient clipping** to bound sensitivity: $g_i \leftarrow g_i / \max(1, \lVert g_i\rVert_2 / C)$.
- **Gaussian noise on the averaged batch gradient**: $\tilde{g} \leftarrow \frac{1}{B}\sum_{i=1}^{B} g_i + \mathcal{N}(0,\sigma^2 C^2 I)$.
- **Poisson subsampling** of mini-batches (privacy amplification), sampling rate $q=B/n$.
- **Tight privacy accounting** via the moments accountant / [[Renyi Differential Privacy (RDP)]], not naive composition.

## Comparison to Previous
| Feature | DP-SGD | Output / Objective Perturbation |
|---------|--------|----------------------------------|
| Noise injection point | Clipped gradients each step | Final params / loss objective |
| Model class | Non-convex (deep) OK | Best in convex settings |
| Convergence | Stationary point (nonconvex) | Can reach global optimum (convex) |

## Training Details
- Dataset: any; utility improves with large $n$.
- Compute: per-example clipping adds overhead (limits hardware batching).
- Techniques: adaptive clipping (Andrew et al. 2021), per-layer clipping (McMahan et al. 2018/2019), moments accountant, RDP/PLD/GDP accounting.

## Strengths & Weaknesses
**Strengths:** flexible, applies to non-convex deep models, tight accounting, widely tooled, (nearly) optimal for finding stationary points under DP.
**Weaknesses:** privacy–utility trade-off, heavy compute from per-example clipping, high hyperparameter sensitivity ($\sigma$, $C$), needs large data.

## Key Papers
- [[A Comprehensive Guide to Differential Privacy]]

## Related
- [[Renyi Differential Privacy (RDP)]]
- [[Gaussian Mechanism]]
- [[Sensitivity]]
- [[Composition]]
