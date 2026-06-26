---
title: Best-of-N
tags: [glossary]
---

## Definition
**Best-of-N** — a test-time search method: sample N candidate outputs from a generator, score each with a [[Reward Model]] (or verifier), and keep the single highest-scored one.

## Context
The standard way to evaluate a reward model's usefulness — a better RM picks the correct candidate more often as N grows. In [[Let's Verify Step by Step]], the [[PRM]]'s advantage over the [[ORM]] and majority voting *widens* with N (reported up to best-of-1860). Also called rejection sampling when used to filter generations.

## See Also
- [[Reward Model]]
- [[PRM]]
- [[ORM]]
