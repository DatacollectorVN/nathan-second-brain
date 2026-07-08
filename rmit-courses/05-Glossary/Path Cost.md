---
title: Path Cost
tags: [glossary]
---

## Definition
> The total cost accumulated along the path from the initial state to node n, written **g(n)** — the sum of the step costs of the actions taken to reach n.

## Context
g(n) is what [[Uniform-Cost Search]] orders its [[Frontier]] by, and one of the two terms in A*'s [[Evaluation Function]] f(n) = g(n) + h(n). [[Greedy Best-First Search]]'s key weakness is that it ignores g(n) entirely, which is why it can be lured down an expensive path that merely looks close to the goal.

## See Also
- [[Evaluation Function]]
- [[Heuristic Function]]
- [[Uniform-Cost Search]]
- [[A* Search]]
- [[AI Lecture 02 — Solving Problems by Searching]]
