---
title: Expectiminimax
family: adversarial-search
tags: [architecture]
status: draft
year:
params:
context_length:
date_added: 2026-07-15
---

## Overview
> Extension of [[Minimax Search]] to stochastic games (dice, card deals, coin tosses) by adding chance nodes: ExpectMinimax(s) takes the max at MAX nodes, min at MIN nodes, and the probability-weighted sum Σ_r P(r)·ExpectMinimax(Result(s, r)) at CHANCE nodes. Introduced in AI Lecture 03 via backgammon.

## Key Design Choices
- Chance nodes model non-deterministic events in the game tree
- Complicates [[Alpha-Beta Pruning]]
- Monte Carlo (random) choices over a large enough sample as an alternative — links to the [[Monte Carlo Simulation]] course

## Comparison to Previous
| Feature | Expectiminimax | Minimax |
|---------|----------------|---------|
| Node types | MAX, MIN, CHANCE | MAX, MIN |
| Value at new node type | Expected value over outcomes | — |

## Training Details
- Dataset: n/a
- Compute: larger trees (chance branching); sampling variants trade exactness for tractability
- Techniques: chance nodes, Monte Carlo sampling

## Strengths & Weaknesses
**Strengths:** handles stochastic games (backgammon, Monopoly) correctly in expectation.
**Weaknesses:** pruning is harder; tree size grows with chance outcomes.

## Key Documents
- [[AI Lecture 03 — Adversarial Search]]

## Related
- [[Minimax Search]]
- [[Alpha-Beta Pruning]]
- [[Monte Carlo Simulation]]
