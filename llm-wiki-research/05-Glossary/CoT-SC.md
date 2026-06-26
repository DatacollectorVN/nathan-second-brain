---
title: CoT-SC
tags: [glossary, reasoning]
---

## Definition
**CoT-SC (Chain-of-Thought Self-Consistency)** — a reasoning decoder that samples multiple [[Chain-of-Thought]] paths with temperature and returns the **majority-vote** answer, rather than a single greedy chain (Wang et al., 2022).

## Context
A strong, training-free reasoning baseline. In [[ReAct - Synergizing Reasoning and Acting in Language Models|ReAct]], CoT-SC uses **21** samples at temperature 0.7 and consistently beats plain CoT. See the full concept note for mechanism and variants.

## See Also
- [[Self-Consistency]]
- [[Chain-of-Thought]]
- [[CoT]]
