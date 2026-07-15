---
title: Adversarial Search
tags: [concept]
status: draft
related_documents:
  - "[[AI Lecture 03 — Adversarial Search]]"
date_added: 2026-07-15
---

## Definition
> Search in multi-agent settings where the goals of different players conflict — game playing formulated as a search problem.

## Intuition
> Unlike single-agent search (8-puzzle), your plan must survive an opponent actively working against you. Instead of building the full game tree, search from the current state for the best next move, let the opponent reply, then search again — repeating until the game ends.

## How It Works
> A game is a search problem defined by: initial state s0, Player(s), Actions(s), Result(s, a), Terminal-test(s), and Utility(s, p) (e.g. −1/0/1 for zero-sum chess). Solved with [[Minimax Search]], made efficient with [[Alpha-Beta Pruning]], depth-limited via heuristic evaluation functions, and extended to chance with [[Expectiminimax]].

## Variants & Depth
> Deterministic vs stochastic × perfect vs imperfect information (chess/Go vs Kriegspiel vs backgammon vs bridge). Stochastic games introduce chance nodes; Monte Carlo sampling links to the Monte Carlo Simulation course.

## Key Documents
- [[AI Lecture 03 — Adversarial Search]]

## Related Concepts
- [[Search Problem]]
- [[State Space Search]]
- [[Minimax Search]]
- [[Alpha-Beta Pruning]]

## My Notes
