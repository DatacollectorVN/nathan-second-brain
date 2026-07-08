---
title: Branching Factor
tags: [glossary]
---

## Definition
> The number of successors of a node — i.e. how many child nodes each expansion generates. Denoted **b** in complexity analysis (the maximum, or average, number of actions available per state).

## Context
b is one of the three parameters used throughout the lecture's performance analysis, alongside [[Solution Depth]] (d) and [[Maximum Path Length]] (m). Time and space costs are expressed in terms of it: BFS is O(b^d), DFS is O(b^m) time / O(bm) space. Completeness of [[Breadth-First Search]] requires b to be finite.

## See Also
- [[Solution Depth]]
- [[Maximum Path Length]]
- [[Completeness]]
- [[AI Lecture 02 — Solving Problems by Searching]]
