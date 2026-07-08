---
title: Solution Depth
tags: [glossary]
---

## Definition
> The depth of the shallowest goal node in the search tree. Denoted **d**.

## Context
d parameterises the cost of the strategies that find the shallowest goal: [[Breadth-First Search]] and [[Iterative Deepening Search]] run in O(b^d) time, with IDS achieving O(bd) space. It contrasts with [[Maximum Path Length]] (m), which can be much larger — the reason DFS's O(b^m) can blow past BFS's O(b^d).

## See Also
- [[Branching Factor]]
- [[Maximum Path Length]]
- [[AI Lecture 02 — Solving Problems by Searching]]
