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

**Step 1 — the distance measure.** Rényi divergence of order $\alpha>1$ between distributions $P$ and $Q$:
$$D_\alpha(P\Vert Q)=\frac{1}{\alpha-1}\log \mathbb{E}_{x\sim Q}\left[\left(\frac{P(x)}{Q(x)}\right)^{\alpha}\right].$$

- $P, Q$ — here, the mechanism's output distributions on two neighboring datasets: $P=\mathcal{A}(D)$, $Q=\mathcal{A}(D')$.
- $P(x)/Q(x)$ — the *likelihood ratio* at output $x$: how much more this particular output points to $D$ than $D'$. This is exactly the quantity a membership-inference adversary computes.
- Raising it to the power $\alpha$ and averaging — instead of taking the single worst-case ratio (as pure DP does), we average the ratio's $\alpha$-th moment. Larger $\alpha$ weights the tail (rare, highly revealing outputs) more heavily; $\alpha\to\infty$ recovers the worst case (pure DP), $\alpha\to 1$ recovers the average case (KL divergence). So $\alpha$ interpolates between "average leakage" and "worst-case leakage."
- $\frac{1}{\alpha-1}\log(\cdot)$ — normalization that puts the moment back on an $\varepsilon$-like log scale so values are comparable across orders. (Note: the DP survey's Def. 10 prints $\frac{1}{1-\alpha}$ — a sign typo; Mironov's original is $\frac{1}{\alpha-1}$, which keeps the divergence non-negative.)

**Step 2 — the privacy definition.** $\mathcal{A}$ is $(\alpha,\varepsilon)$-RDP if for all neighbors $D\sim D'$:
$$D_\alpha(\mathcal{A}(D)\Vert\mathcal{A}(D'))\le\varepsilon.$$
Same shape as DP — "outputs on neighboring datasets are close" — but closeness is measured by a moment of the likelihood ratio rather than a uniform bound on it.

**Step 3 — why this composes so well.** Run $k$ mechanisms, each $(\alpha,\varepsilon_i)$-RDP: the total is $(\alpha, \sum_i \varepsilon_i)$-RDP — *exactly additive at fixed $\alpha$*, with none of the $\sqrt{k}$-vs-$k$ slack juggling of advanced composition. Moments of the privacy-loss variable simply add across independent steps. This is what makes tracking thousands of DP-SGD iterations tractable.

**Step 4 — converting back to $(\varepsilon,\delta)$-DP.** At the end you report a standard guarantee via the tail bound
$$\delta=\min_\lambda\exp(\alpha_\mathcal{M}(\lambda)-\lambda\varepsilon),$$
where $\alpha_\mathcal{M}(\lambda)$ is the accumulated log-moment of the privacy loss at order $\lambda$ (a Chernoff/Markov bound: the probability that the realized privacy loss exceeds $\varepsilon$). The accountant computes $\alpha_\mathcal{M}(\lambda)$ for a grid of orders, sums over iterations, and picks the order giving the smallest $\delta$ (or smallest $\varepsilon$ for a target $\delta$).

**Worked payoff (Example 17 of the survey):** for DP-SGD with sampling rate $q=0.01$, $\sigma=4$, $\delta=10^{-5}$: after 100 epochs, advanced composition gives $\varepsilon=9.34$ but moments/RDP accounting gives $\varepsilon=1.26$ — a ~7× tighter budget for identical training.

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
