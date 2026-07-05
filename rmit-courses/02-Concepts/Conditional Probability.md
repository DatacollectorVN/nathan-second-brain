---
title: Conditional Probability
tags: [concept]
status: draft
related_documents:
  - "[[Statistics and Probability Foundation]]"
date_added: 2026-07-05
---

## Definition
> The probability of event A given that event B has already happened: P(A|B) = P(A∩B) / P(B) — the "intersection of dependent events".

## Intuition
> New information shrinks the sample space. Knowing a drawn card (from 10 number cards) is odd reduces the space from 10 to 5 cards, so P(card = 5) jumps from 1/10 to 1/5. If A is independent of B, the information changes nothing: P(A|B) = P(A).

## How It Works
Three equivalent approaches from the course: the formula P(A|B) = P(A∩B)/P(B); reducing the sample space (B becomes the new denominator); and Venn diagrams (intersection area divided by the whole B area).

## Variants & Depth
Business examples: P(purchase A | already purchased B), P(default | sector), P(late task | specific resource assigned), P(breakdown | missed maintenance). Foundation for Bayes-style reasoning — likely to recur in ML courses.

## Key Documents
- [[Statistics and Probability Foundation]]

## Related Concepts
- [[Probability Distribution]]
- [[Random Variable]]

## My Notes
