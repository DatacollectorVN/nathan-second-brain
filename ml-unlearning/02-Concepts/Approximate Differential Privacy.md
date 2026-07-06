---
title: Approximate Differential Privacy
tags: [concept, privacy, certified]
status: draft
related_papers:
  - A Comprehensive Guide to Differential Privacy
date_added: 2026-07-06
---

## Definition
> One-sentence definition.

Approximate, or $(\varepsilon,\delta)$-, differential privacy relaxes pure $\varepsilon$-DP by allowing a small failure probability $\delta$ that the multiplicative privacy bound may not hold.

## Intuition
> Explain it simply, as if to a colleague.

Pure $\varepsilon$-DP is often too strict (e.g., Gaussian noise can't satisfy it). Allowing a tiny "bad event" probability $\delta$ makes the framework usable in ML while keeping the guarantee essentially strong, as long as $\delta$ is kept negligibly small.

## How It Works
> Technical detail, math if applicable.

For $\varepsilon>0$, $\delta\in[0,1]$, and neighbors $D\sim D'$:
$$P[\mathcal{A}(D)\in S] \le \exp(\varepsilon)\cdot P[\mathcal{A}(D')\in S] + \delta.$$

**What each symbol means**

- $\mathcal{A}$ — the *mechanism*: a randomized algorithm (noisy query, DP-SGD training run…) whose output we're protecting.
- $D \sim D'$ — *neighboring datasets*: identical except for one person's record. The definition compares the world *with* you to the world *without* you.
- $S$ — any set of possible outputs. We bound probabilities of output *sets* (not single values) because the output is a random variable; the bound must hold for **every** $S$, so an adversary can't find any observable event that betrays your presence.
- $\exp(\varepsilon)$ — the multiplicative slack: any output event may be at most $e^\varepsilon$ times more likely with your record than without. Small $\varepsilon$ ⇒ ratio ≈ 1 ⇒ the two worlds look alike.
- $\delta$ — an *additive* escape hatch: the probability mass on which the multiplicative guarantee is allowed to fail entirely.

**The logic.** Pure $\varepsilon$-DP demands the ratio bound hold for *every* event, including astronomically rare ones. Gaussian noise can't satisfy that — its tails decay too fast, so for extreme outputs the likelihood ratio blows up. $(\varepsilon,\delta)$-DP says: enforce the ratio bound *except* on a bad event of probability ≤ $\delta$. Read it as "with probability ≥ $1-\delta$, you get $\varepsilon$-DP; with probability ≤ $\delta$, no promise."

**Why $\delta \ll 1/n$.** Worst-case reading: a mechanism could use its $\delta$-budget to fully expose a random record. With $n$ records, the expected number of exposed individuals is $\approx \delta n$; requiring $\delta \ll 1/n$ keeps that expectation well below one person. This is why $\delta = 10^{-5}$ is common for datasets of ~10⁴–10⁵ records.

$\delta=0$ recovers pure DP, so pure DP is the special case, not a different framework.

## Variants & Evolution
> How has this concept evolved across papers/models?

Motivated tighter accountants — [[Renyi Differential Privacy (RDP)]], zCDP, Gaussian DP — that track loss more precisely and convert back to $(\varepsilon,\delta)$ bounds. It is the standard target for [[DP-SGD]].

## Key Papers
- [[A Comprehensive Guide to Differential Privacy]]

## Related Concepts
- [[Differential Privacy]]
- [[Renyi Differential Privacy (RDP)]]
- [[Composition]]

## My Notes
$(\varepsilon,\delta)$ is the guarantee form most certified-unlearning papers adopt when bounding how close an unlearned model is to a retrained one.
