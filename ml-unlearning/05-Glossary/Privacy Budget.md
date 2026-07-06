---
title: Privacy Budget
tags: [glossary, privacy]
---

## Definition
> Short, precise definition (1-2 sentences).

The privacy budget is the parameters $\varepsilon$ (and $\delta$) that cap the total allowed privacy loss. Each query or training step spends part of it; higher accuracy/utility costs more budget.

## Context
> When and how is this term used?

Managed via [[Composition]] theorems — sequential composition sums $\varepsilon$ and $\delta$; advanced/RDP accounting gives tighter spend. Analogous to *deletion capacity* in certified unlearning.

## See Also
- [[Composition]]
- [[(epsilon-delta)-DP]]
- [[Renyi Differential Privacy (RDP)]]
