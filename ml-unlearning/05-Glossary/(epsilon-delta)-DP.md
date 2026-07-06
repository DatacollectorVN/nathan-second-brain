---
title: (ε,δ)-DP
tags: [glossary, privacy]
---

## Definition
> Short, precise definition (1-2 sentences).

$(\varepsilon,\delta)$-DP (approximate DP) requires $P[\mathcal{A}(D)\in S]\le e^{\varepsilon}P[\mathcal{A}(D')\in S]+\delta$ for all neighboring datasets $D\sim D'$. $\varepsilon$ bounds the multiplicative privacy loss; $\delta$ is the small probability the bound may fail.

## Context
> When and how is this term used?

The standard guarantee for machine-learning DP (e.g., [[DP-SGD]]), where pure $\varepsilon$-DP is infeasible. Guideline: keep $\delta \ll 1/n$.

## See Also
- [[Approximate Differential Privacy]]
- [[Privacy Budget]]
- [[Differential Privacy]]
