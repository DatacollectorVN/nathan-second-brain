---
title: Minimax Search
family: adversarial-search
tags: [architecture]
status: draft
year:
params:
context_length:
date_added: 2026-07-15
---

## Overview
> Depth-first game-tree search for two-player zero-sum games: MAX maximises, MIN minimises, assuming a perfectly-playing (infallible) opponent. Minimax(s) = Utility(s) at terminals, max_a at MAX nodes, min_a at MIN nodes. Introduced in AI Lecture 03.

## Key Design Choices
- Depth-first computation; depth bounded via ply (2n-ply = n moves each)
- Assumes optimal opponent; root is a MAX node with alternating MAX/MIN levels
- Time O(b^m) — motivates [[Alpha-Beta Pruning]] and depth cutoff with an [[Evaluation Function]] (H-Minimax)

## Comparison to Previous
| Feature | Minimax | Single-agent search (Week 2) |
|---------|---------|------------------------------|
| Agents  | Two, adversarial | One |
| Node value | Alternating max/min of children | Path cost / heuristic f(n) |

## Training Details
- Dataset: n/a (classical search algorithm)
- Compute: O(b^m) time, depth-first space
- Techniques: ply cutoff, [[Alpha-Beta Pruning]], heuristic evaluation

## Strengths & Weaknesses
**Strengths:** optimal against a perfect opponent; simple recursive definition.
**Weaknesses:** exponential time; infeasible for full trees of real games (chess ≈ 10^40 states); assumes infallible opponent.

## Key Documents
- [[AI Lecture 03 — Adversarial Search]]

## Related
- [[Adversarial Search]]
- [[Alpha-Beta Pruning]]
- [[Expectiminimax]]
