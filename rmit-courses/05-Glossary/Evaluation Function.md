---
title: Evaluation Function
tags: [glossary]
---

## Definition
> The function a best-first search uses to rank the [[Frontier]] and decide which node to expand next. In [[A* Search]] it is **f(n) = g(n) + h(n)** — path cost so far plus estimated cost to the goal.

## Context
The choice of evaluation function defines the informed strategy: f(n) = h(n) alone gives [[Greedy Best-First Search]]; f(n) = g(n) alone gives [[Uniform-Cost Search]]; f(n) = g(n) + h(n) gives [[A* Search]]. For A*, f(n) is the estimated cost of the cheapest solution passing through n, and it is non-decreasing along a path when h is a [[Consistent Heuristic]].

## See Also
- [[Path Cost]]
- [[Heuristic Value]]
- [[A* Search]]
- [[AI Lecture 02 — Solving Problems by Searching]]
