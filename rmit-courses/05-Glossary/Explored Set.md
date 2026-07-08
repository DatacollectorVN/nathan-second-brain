---
title: Explored Set
tags: [glossary]
---

## Definition
> The set of states that have already been expanded, kept (typically in a hash table) so the search does not re-expand a repeated state. Also called the closed list.

## Context
Adding an explored set turns tree-search into graph-search: a newly generated node is added to the [[Frontier]] only if it is not already in the frontier or the explored set. This removes the redundancy of repeated states (e.g. oscillations in the 8-Puzzle, many paths to the same grid cell) — "algorithms that cannot remember the past are condemned to repeat it."

## See Also
- [[Frontier]]
- [[State Space Search]]
- [[AI Lecture 02 — Solving Problems by Searching]]
