---
title: Search Problem
tags: [concept]
status: draft
related_documents:
  - "[[AI Lecture 01 — Introduction to Artificial Intelligence]]"
date_added: 2026-07-01
---
	
## Definition
> A problem defined by six components — possible states, initial state, actions, transition model, goal test, and path cost — whose solution is a sequence of actions transforming the initial state into a goal state.

## Intuition
Many AI tasks (path finding, games, puzzles) can be cast in one common shape: describe the states you can be in, where you start, what you can do, what each action leads to, how to recognise success, and what each path costs. Solve it by exploring reachable states.

## How It Works
The six components:
1. **Possible states** — the state space.
2. **Initial state** — where search begins.
3. **Actions** — available operations.
4. **Transition model** — result of applying an action to a state.
5. **Goal test** — whether a state satisfies the goal.
6. **Path cost** — cost accumulated along a path.

Solving reduces to [[State Space Search]] / graph reachability: is a goal node reachable from the initial node?

## Variants & Depth
> Illustrated with Water Jugs, 8-Puzzle, N-Queens, and Vacuum World. Sets up uninformed and informed (heuristic) search algorithms in following weeks.

## Key Documents
- [[AI Lecture 01 — Introduction to Artificial Intelligence]]

## Related Concepts
- [[State Space Search]]

## My Notes
