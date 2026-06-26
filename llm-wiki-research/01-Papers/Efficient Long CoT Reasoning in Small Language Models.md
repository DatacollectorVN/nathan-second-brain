---
title: Efficient Long CoT Reasoning in Small Language Models
authors: Zhaoyang Wang, Jinqi Jiang, Tian Qiu, Hui Liu, Xianfeng Tang, Huaxiu Yao
year: 2025
arxiv: https://arxiv.org/abs/2505.18440
tags:
  - paper
  - SLM
  - CoT
  - distillation
  - reasoning
  - efficiency
status: reviewed
topic:
  - Small Language Models
  - Chain-of-Thought Reasoning
  - Knowledge Distillation
  - Efficient Inference
related_concepts:
  - "[[Chain-of-Thought]]"
  - "[[Knowledge Distillation]]"
  - "[[SFT]]"
  - "[[DPO]]"
  - "[[On-Policy Learning]]"
date_added: 2025-06-13
---

## Summary
Large reasoning models like DeepSeek-R1 generate long chain-of-thought (CoT) steps that are effective but contain significant redundancy (overthinking). Directly distilling these bloated traces into Small Language Models (~7B params) is harmful because SLMs have limited capacity and struggle to filter out irrelevant steps. This paper proposes a **binary cutting + on-policy validation** pipeline to prune redundant CoT steps before distillation, producing concise, SLM-tailored training data. The result: competitive reasoning performance with significantly fewer generated tokens.

---

## Key Contributions
- Identified that redundant/overthinking steps in long CoT **hurt** SLM distillation, not just inference efficiency
- Proposed **Binary Cutting** — an O(log₂ n) algorithm to find the shortest valid CoT prefix that still leads to the correct answer, replacing the naive O(n) linear search
- Proposed **On-Policy Validation** — uses the *target SLM itself* as the validator, so pruned CoT aligns with that model's own reasoning capacity (not an external judge)
- Combined SFT + DPO fine-tuning on the pruned dataset for efficient long CoT reasoning

---

## Architecture / Method

### Pipeline (3 stages)
```
Large Reasoning Model (DeepSeek-R1)
        ↓ generates long CoT traces
  [Stage 1] Response Sampling
        ↓ split thinking into steps [s1, s2, ..., sn]
  [Stage 2] Binary Cutting + Backtracking
        ↓ find shortest prefix T^{1:k} where SLM still answers correctly
  [Stage 3] On-Policy Validation
        ↓ SLM itself validates each candidate prefix
  Distilled Dataset (concise CoT)
        ↓
  SFT → DPO fine-tuning on target SLM
```

### Binary Cutting (key algorithm)
- Start: `low=1, high=n`
- Each iteration: `mid = floor((low+high)/2)`, test prefix `T^{1:mid}`
- If valid → shrink (`high=mid`); if invalid → **backtrack** (search upward from last mid)
- Complexity: **O(log₂ n)** vs O(n) for linear search
- Backtracking recovers over-pruned steps — critical because 61.83% of samples have the answer in the last 10 steps

### On-Policy Validation
- Validator = target SLM itself (not an external judge)
- Prompt: *"Give the final answer based only on these thinking steps"*
- Ensures distilled CoT aligns with SLM's **own reasoning biases and strengths**
- Key insight: different SLMs have different inductive preferences

### Fine-tuning
- **SFT**: maximize likelihood of pruned response Y given question Q
- **DPO**: treat pruned Y as "good", original bloated R as "bad" — teaches preference for concise reasoning
- DPO loss augmented with SFT loss (weight=0.3) for training stability

---

## Results & Benchmarks

| Model | Method | GSM8K Acc | GSM8K Tokens | MATH Acc | MATH Tokens | AIME Acc | AIME Tokens |
|-------|--------|-----------|--------------|----------|-------------|----------|-------------|
| Llama-3.1-8B | Base | 76.80% | 232 | 37.80% | 2454 | 12.97% | 6204 |
| Llama-3.1-8B | SFT Full | 89.01% | 1051 | 54.80% | 3274 | 16.51% | 7262 |
| Llama-3.1-8B | **SFT Ours** | 87.34% | **502 (↓52%)** | 54.00% | **2322 (↓29%)** | **18.01%** | 5480 |
| Llama-3.1-8B | **SFT+DPO Ours** | 87.41% | **339 (↓68%)** | 52.40% | **1324 (↓60%)** | 17.90% | 3779 |
| Qwen2.5-7B | SFT Full | 90.37% | 1011 | 64.00% | 2712 | 28.51% | 6330 |
| Qwen2.5-7B | **SFT Ours** | 89.16% | **382 (↓62%)** | 61.20% | **901 (↓67%)** | 25.51% | 3021 |
| Qwen2.5-7B | **SFT+DPO Ours** | 89.92% | **278 (↓73%)** | 56.60% | **489 (↓82%)** | 21.54% | 1836 |

**Key finding**: ~40% of long CoT data can be streamlined by removing >50% of tokens with minimal accuracy loss.

---

## Limitations
- Binary cutting finds a *good* prefix but not guaranteed globally optimal subset of reasoning steps
- Only evaluates on 7B-scale models (most common community size, but not comprehensive)
- Focused on distillation scenario — does not explore RL or self-training for SLMs
- Experiments only on mathematical reasoning benchmarks (GSM8K, MATH, AIME)

---

## My Notes & Questions
- The on-policy trick is elegant — using the target model as its own validator is a form of **self-play** that naturally adapts the training data to model capability
- DPO stage contributes most to token reduction but hurts hard tasks (AIME) — interesting tension between efficiency and peak performance
- Worth exploring: does this approach generalize to **non-math** domains (code, science QA)?
- Related to my RMIT research interest in SLMs for agentic tool use — could pruned CoT traces help SLMs do more efficient tool-calling chains?
- Dataset used: **OpenR1-Math-220k** (93.7k train samples → ~25k valid pruned samples after pipeline)

---

## Related
- [[Chain-of-Thought]]
- [[Knowledge Distillation]]
- [[DPO]]
- [[DeepSeek-R1]]
- [[Small Language Models]]

---

## Review
**2026-06-17 — VERDICT: PASS** (verified against arXiv 2505.18440 v2)

| #   | Check        | Result                                                                                                                                                                                                                                          |
| --- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Faithfulness | ✅ Every Table-1 number exact (Base/SFT_Full/SFT_Ours/SFT+DPO_Ours for both Llama-3.1-8B & Qwen2.5-7B); token-reduction %s match the paper's own subscripts; 61.83%, ~40%/>50%, O(log₂n), SFT-weight 0.3, OpenR1-Math-220k 93.7k→25k all correct |
| 2   | Completeness | ✅ All sections filled, no leftover placeholders, frontmatter complete                                                                                                                                                                           |
| 3   | Wikilinks    | ✅ All resolve; ⚠️ minor: `[[Overthinking in Reasoning Models]]`, `[[Llama-3.1-8B-Instruct]]`, `[[Qwen2.5-7B-Instruct]]` exist in vault and are named in prose but not linked                                                                    |
| 4   | Conventions  | ✅ Tags valid; correct folder                                                                                                                                                                                                                    |
| 5   | Cross-note   | ✅ Auto-indexed by Papers-MOC Dataview                                                                                                                                                                                                           |
| 6   | Calibration  | ✅ Limitations match source; My Notes clearly separated as opinion                                                                                                                                                                               |

Optional follow-up: add the three wikilinks in #3. Non-blocking.
