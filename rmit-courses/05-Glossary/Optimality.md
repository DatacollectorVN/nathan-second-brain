---
title: Optimality
tags: [glossary]
---

## Definition
> A property of a search algorithm: it is **optimal** if the solution it returns is guaranteed to be the one with the lowest [[Path Cost]].

## Context
One of the four performance criteria (with [[Completeness]], time, and space). [[Breadth-First Search]] is optimal only when all step costs are equal; [[Uniform-Cost Search]] is optimal for any non-negative step costs; [[A* Search]] is optimal when its heuristic is [[Admissible Heuristic|admissible]] (tree-search) or [[Consistent Heuristic|consistent]] (graph-search). [[Depth-First Search]] and [[Greedy Best-First Search]] are not optimal.

## See Also
- [[Completeness]]
- [[Path Cost]]
- [[AI Lecture 02 — Solving Problems by Searching]]
