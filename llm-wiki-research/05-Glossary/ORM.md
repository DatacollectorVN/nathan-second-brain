---
title: ORM
tags: [glossary]
---

## Definition
**ORM (Outcome-supervised Reward Model)** — a [[Reward Model]] trained to predict whether a *whole solution* is correct, using only the final result; the score is read from the prediction at the solution's final token.

## Context
Trained via [[Outcome Supervision]], usually with automatic final-answer checking. Cheaper than a [[PRM]] but susceptible to false positives (right answer via wrong reasoning). In [[Let's Verify Step by Step]] the ORM reached 72.4% on the [[MATH]] subset vs the PRM's 78.2%.

## See Also
- [[PRM]]
- [[Outcome Supervision]]
- [[Reward Model]]
