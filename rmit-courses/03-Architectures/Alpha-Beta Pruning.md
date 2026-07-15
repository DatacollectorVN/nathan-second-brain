---
title: Alpha-Beta Pruning
family: adversarial-search
tags: [architecture]
status: draft
year:
params:
context_length:
date_added: 2026-07-15
---

## Overview
> Optimisation of [[Minimax Search]] that skips subtrees which cannot affect the root decision. α = highest current MAX value on the path to the root; β = lowest current MIN value on that path. Stop expanding a MIN node once its value ≤ parent's α, and a MAX node once its value ≥ parent's β. Same result as minimax, fewer expansions.

## Key Design Choices
- Identical outcome to minimax — pruning is lossless
- Effectiveness depends on move ordering ("generate children in desired order")
- Best case O(b^(m/2)) vs minimax O(b^m): effective branching factor √b — twice the depth in the same time
- Implementation trick: transposition tables as hash tables

## Comparison to Previous
| Feature | Alpha-Beta | Minimax |
|---------|-----------|---------|
| Time (best ordering) | O(b^(m/2)) | O(b^m) |
| Result | Identical | — |

## Training Details
- Dataset: n/a
- Compute: order-dependent; O(b^(m/2)) best case
- Techniques: move ordering, [[Transposition Table]]

## Strengths & Weaknesses
**Strengths:** dramatic speedup with no loss of decision quality.
**Weaknesses:** gains depend heavily on expansion order; chance nodes complicate pruning ([[Expectiminimax]]).

## Key Documents
- [[AI Lecture 03 — Adversarial Search]]

## Related
- [[Minimax Search]]
- [[Adversarial Search]]
