---
title: HotpotQA
tags: [glossary, benchmark]
---

## Definition
**HotpotQA** (Yang et al., 2018) — a multi-hop question-answering benchmark whose questions require reasoning over **two or more Wikipedia passages**. Scored by exact match (EM).

## Context
Used in [[ReAct - Synergizing Reasoning and Acting in Language Models|ReAct]] in a *question-only* setup (no supporting paragraphs), forcing retrieval via a Wikipedia API. ReAct's best prompting result is 35.1 EM (ReAct → CoT-SC) vs supervised SoTA 67.5.

## See Also
- [[FEVER]]
- [[Chain-of-Thought]]
