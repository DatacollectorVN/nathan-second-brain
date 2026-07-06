---
title: Rényi Differential Privacy
tags: [concept, privacy]
status: draft
related_papers:
  - A Comprehensive Guide to Differential Privacy
date_added: 2026-07-06
---

## Definition
> One-sentence definition.

Rényi differential privacy (RDP), introduced by Mironov (2017), quantifies privacy loss via the Rényi divergence between a mechanism's output distributions on neighboring datasets, giving tighter, additively-composable bounds than $(\varepsilon,\delta)$-DP.

## Intuition
> Explain it simply, as if to a colleague.

Instead of a single worst-case ratio, RDP measures distinguishability at a chosen "order" $\alpha$, which grows gracefully under composition. This is what makes the moments accountant able to track privacy loss tightly across thousands of [[DP-SGD]] steps.

## How It Works
> Technical detail, math if applicable.

Rényi divergence of order $\alpha>1$:
$$D_\alpha(P\Vert Q)=\frac{1}{\alpha-1}\log \mathbb{E}_{x\sim Q}\left[\left(\frac{P(x)}{Q(x)}\right)^{\alpha}\right].$$
A mechanism is $(\alpha,\varepsilon)$-RDP if $D_\alpha(\mathcal{A}(D)\Vert\mathcal{A}(D'))\le\varepsilon$ for all neighbors. RDP composes by simply adding $\varepsilon$ at fixed $\alpha$, then converts to $(\varepsilon,\delta)$-DP via a tail bound $\delta=\min_\lambda\exp(\alpha_\mathcal{M}(\lambda)-\lambda\varepsilon)$.

## Variants & Evolution
> How has this concept evolved across papers/models?

Underlies the **moments accountant** (Abadi et al. 2016). Related concentrated-privacy notions: zCDP (Bun et al. 2016) and Gaussian DP (Dong et al. 2022); Asoodeh et al. (2020) give an optimal RDP→$(\varepsilon,\delta)$ conversion; PLD accounting (Koskela et al. 2020) tracks the full loss distribution.

## Key Papers
- [[A Comprehensive Guide to Differential Privacy]]

## Related Concepts
- [[Approximate Differential Privacy]]
- [[Composition]]
- [[DP-SGD]]

## My Notes
RDP-style accounting is the natural tool for bounding *sequential deletion* budgets in certified unlearning — the same additive composition intuition.
