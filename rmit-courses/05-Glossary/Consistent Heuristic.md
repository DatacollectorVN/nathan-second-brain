---
title: Consistent Heuristic
tags: [glossary]
---

## Definition
> A [[Heuristic Function|heuristic]] satisfying the triangle-inequality condition h(n) ≤ cost(n, n′) + h(n′) for every node n and successor n′ (also called monotonic). Consistency implies [[Admissible Heuristic|admissibility]].

## Context
Consistency is the stronger property required for graph-search [[A* Search]] to remain optimal (plain admissibility only guarantees optimality for tree-search). Under a consistent heuristic, f(n) is non-decreasing along any path. The lecture notes that almost any admissible heuristic met in practice is also consistent.

## See Also
- [[Admissible Heuristic]]
- [[Heuristic Function]]
- [[A* Search]]
- [[AI Lecture 02 — Solving Problems by Searching]]
