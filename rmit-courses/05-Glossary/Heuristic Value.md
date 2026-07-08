---
title: Heuristic Value
tags: [glossary]
---

## Definition
> **h(n)** — the value returned by a [[Heuristic Function]] at node n: an estimate of the cheapest cost from n to a goal. Required: h(n) ≥ 0, and h(n) = 0 at any goal node.

## Context
h(n) is the term that makes a search *informed*. [[Greedy Best-First Search]] orders its [[Frontier]] by h(n) alone; [[A* Search]] uses it inside the [[Evaluation Function]] f(n) = g(n) + h(n). Whether A* is optimal depends on properties of h — see [[Admissible Heuristic]] and [[Consistent Heuristic]]. Full treatment (relaxation, informedness, 8-Puzzle examples) is in [[Heuristic Function]].

## See Also
- [[Heuristic Function]]
- [[Path Cost]]
- [[Evaluation Function]]
- [[Admissible Heuristic]]
- [[AI Lecture 02 — Solving Problems by Searching]]
