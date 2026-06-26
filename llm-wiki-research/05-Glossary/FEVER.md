---
title: FEVER
tags: [glossary, benchmark]
---

## Definition
**FEVER** (Fact Extraction and VERification; Thorne et al., 2018) — a fact-verification benchmark where each claim is labeled **SUPPORTS, REFUTES, or NOT ENOUGH INFO** based on whether a Wikipedia passage verifies it. Scored by accuracy.

## Context
Used in [[ReAct - Synergizing Reasoning and Acting in Language Models|ReAct]] in a question/claim-only setup. ReAct beats CoT here (60.9 vs 56.3 Acc); best prompting result is 64.6 (CoT-SC → ReAct) vs supervised SoTA 89.5.

## See Also
- [[HotpotQA]]
