---
title: Maximum Path Length
tags: [glossary]
---

## Definition
> The length of the longest path (maximum depth) in the search space. Denoted **m**; can be infinite in spaces with cycles or unbounded depth.

## Context
m governs [[Depth-First Search]]'s worst-case time O(b^m) and space O(bm). Because m can be much larger than [[Solution Depth]] (d) — or infinite — DFS is neither complete nor optimal in general, which is what [[Depth-Limited Search]] (bound l) and [[Iterative Deepening Search]] are designed to tame.

## See Also
- [[Branching Factor]]
- [[Solution Depth]]
- [[AI Lecture 02 — Solving Problems by Searching]]
