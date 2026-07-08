---
title: Frontier
tags: [glossary]
---

## Definition
> The set of nodes that have been generated but not yet expanded — the "fringe" of the search tree from which the next node to expand is chosen. Also called the open list.

## Context
The frontier's internal ordering *is* the search strategy: a FIFO queue gives [[Breadth-First Search]], a LIFO stack gives [[Depth-First Search]], a priority queue by g(n) gives [[Uniform-Cost Search]], by h(n) gives [[Greedy Best-First Search]], and by f(n) = g(n) + h(n) gives [[A* Search]]. Space complexity is dominated by how large the frontier grows.

## See Also
- [[Explored Set]]
- [[State Space Search]]
- [[AI Lecture 02 — Solving Problems by Searching]]
