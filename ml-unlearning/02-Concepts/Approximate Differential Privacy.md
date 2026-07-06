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
$\delta=0$ recovers pure DP. Worst-case reasoning: expected successful identifications $\approx \delta n$, so choose $\delta \ll 1/n$.

## Variants & Evolution
> How has this concept evolved across papers/models?

Motivated tighter accountants — [[Renyi Differential Privacy]], zCDP, Gaussian DP — that track loss more precisely and convert back to $(\varepsilon,\delta)$ bounds. It is the standard target for [[DP-SGD]].

## Key Papers
- [[A Comprehensive Guide to Differential Privacy]]

## Related Concepts
- [[Differential Privacy]]
- [[Renyi Differential Privacy]]
- [[Composition]]

## My Notes
$(\varepsilon,\delta)$ is the guarantee form most certified-unlearning papers adopt when bounding how close an unlearned model is to a retrained one.
