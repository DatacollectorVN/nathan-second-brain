---
title: Admissible Heuristic
tags: [glossary]
---

## Definition
> A [[Heuristic Function|heuristic]] h(n) that never overestimates the true cost of the cheapest path from n to a goal (h(n) ≤ true cost, and h(goal) = 0).

## Context
Admissibility is the condition that makes tree-search [[A* Search]] optimal: because h never overestimates, A* cannot skip past the cheapest solution. On the 8-Puzzle both misplaced-tiles (h1) and Manhattan-distance (h2) are admissible. The stronger [[Consistent Heuristic]] condition is what graph-search A* needs.

## See Also
- [[Heuristic Function]]
- [[Consistent Heuristic]]
- [[A* Search]]
- [[AI Lecture 02 — Solving Problems by Searching]]
