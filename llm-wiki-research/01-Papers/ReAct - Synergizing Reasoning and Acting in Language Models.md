---
title: "ReAct: Synergizing Reasoning and Acting in Language Models"
authors: Shunyu Yao, Jeffrey Zhao, Dian Yu, Nan Du, Izhak Shafran, Karthik Narasimhan, Yuan Cao
year: 2023
arxiv: https://arxiv.org/abs/2210.03629
tags:
  - paper
  - agentic
  - reasoning
status: reviewed
topic:
  - Agentic Systems
  - Reasoning + Acting
  - Tool Use
  - LLM Agents
related_concepts:
  - "[[Agentic AI]]"
  - "[[Chain-of-Thought]]"
  - "[[Tool Calling]]"
  - "[[Self-Consistency]]"
date_added: 2026-06-26
---

## Summary
LLM **reasoning** (e.g. [[Chain-of-Thought]]) and **acting** (action/plan generation in environments) had been studied separately. ReAct is a prompting paradigm that interleaves both: the model emits free-form **reasoning traces ("thoughts")** and **task-specific actions** in the same trajectory, so reasoning can build/track/adjust plans and handle exceptions, while actions query external sources (a Wikipedia API, a game, a shopping site) to ground that reasoning. Using a frozen **PaLM-540B** with only a handful of in-context examples, ReAct reduces the hallucination and error-propagation seen in pure CoT on knowledge tasks (HotpotQA, FEVER) and, on interactive decision-making benchmarks (ALFWorld, WebShop), beats imitation/RL agents trained on 10³–10⁵ instances by an absolute **+34%** and **+10%** success rate respectively, while also being more interpretable. (arXiv first posted Oct 2022; published at ICLR 2023.)

## Key Contributions
- **Introduces ReAct**, a prompt-based paradigm that augments the action space with a *language/thought* space, synergizing reasoning and acting in a single LLM trajectory (§2).
- **Few-shot wins on interactive tasks:** one-/two-shot ReAct beats imitation- and RL-trained agents by absolute **+34%** (ALFWorld) and **+10%** (WebShop) success rate (Abstract, §4).
- **Reasoning + retrieval beats reasoning alone:** on HotpotQA/FEVER, combining ReAct with self-consistency CoT (CoT-SC) is the best prompting approach, mitigating CoT's hallucination by grounding in a Wikipedia API (§3.3).
- **Systematic ablations** isolating the value of acting in reasoning tasks and reasoning in acting tasks (incl. the ReAct-IM Inner-Monologue ablation), plus **finetuning** experiments showing ReAct scales better than baselines with modest training data (§3.3, §4).

## Architecture / Method

### Core idea
- Standard agent setup: at step *t* an agent sees observation *oₜ* and takes action *aₜ* under policy π(aₜ|cₜ), with context cₜ = (o₁,a₁,…,oₜ). ReAct **augments the action space to Â = A ∪ L**, where *L* is the space of language (§2).
- A language action **âₜ ∈ L (a "thought")** does not affect the environment and returns no observation; it just composes/updates context (cₜ₊₁ = (cₜ, âₜ)) to support later reasoning or acting. Thought types include goal decomposition, commonsense injection, extracting info from observations, progress tracking, and exception handling (§2).
- Setup is a **frozen PaLM-540B prompted with few-shot in-context exemplars** (human-written action/thought/observation trajectories). For reasoning-heavy tasks thoughts are **dense** (every step); for long-horizon decision tasks thoughts are **sparse** and the model decides when to emit them (§2).

### Knowledge-intensive tasks (HotpotQA, FEVER) — §3
- **Question-only setup:** the model gets only the question/claim and must rely on internal knowledge or retrieval. A simple **Wikipedia API** with three actions: `search[entity]` (returns first 5 sentences of the page, else top-5 similar entities), `lookup[string]` (next sentence containing the string, Ctrl+F-like), `finish[answer]` (§3.1).
- **Exemplars:** 6 (HotpotQA) and 3 (FEVER) manually written ReAct trajectories; the paper notes more examples did not help (§3.2).
- **Baselines** built by ablating ReAct trajectories: Standard, CoT, **CoT-SC** (self-consistency over **21** sampled CoT trajectories, temperature 0.7, majority vote), and Act (no thoughts) (§3.2).
- **Combining internal + external knowledge** via two heuristics: **ReAct → CoT-SC** (back off to CoT-SC if ReAct fails within 7/5 steps for HotpotQA/FEVER), and **CoT-SC → ReAct** (back off to ReAct when the CoT-SC majority answer occurs < n/2 times) (§3.2).
- **Finetuning:** bootstrap **3,000** ReAct-generated correct trajectories to finetune PaLM-8B/62B (§3.2).

### Decision-making tasks (ALFWorld, WebShop) — §4
- **ALFWorld:** text-based embodied game, 6 task types; instances can span >50 locations and >50 expert steps. Evaluated on **134 unseen games**; 3 annotated trajectories/task type, 6 prompts per type via permutations of 2-of-3. Baseline **BUTLER**, an imitation agent trained on **10⁵** expert trajectories per task type (§4).
- **WebShop:** online-shopping env with **1.18M** products and **12k** human instructions; agent must buy a product matching an instruction. Evaluated by **average score** and **success rate** on **500** test instructions. Baselines: **IL** (1,012 human trajectories) and **IL+RL** (+10,587 training instructions) (§4).
- **ReAct-IM ablation:** an Inner-Monologue-style variant using dense external-feedback "thoughts," to isolate the value of flexible internal reasoning vs. reactive feedback (§4).

## Results & Benchmarks

PaLM-540B prompting on knowledge tasks (Table 1, §3.3):

| Method              | HotpotQA (EM) | FEVER (Acc) |
| ------------------- | ------------- | ----------- |
| Standard            | 28.7          | 57.1        |
| CoT                 | 29.4          | 56.3        |
| CoT-SC (21 samples) | 33.4          | 60.4        |
| Act                 | 25.7          | 58.9        |
| ReAct               | 27.4          | 60.9        |
| CoT-SC → ReAct      | 34.2          | 64.6        |
| ReAct → CoT-SC      | 35.1          | 62.0        |
| Supervised SoTA     | 67.5          | 89.5        |

- ReAct **> Act** on both tasks; ReAct **> CoT on FEVER (60.9 vs 56.3)** but **slightly < CoT on HotpotQA (27.4 vs 29.4)** (§3.3).
- Best prompting methods: **ReAct → CoT-SC (35.1 EM)** on HotpotQA, **CoT-SC → ReAct (64.6 Acc)** on FEVER; both reach 21-sample CoT-SC quality with just **3–5** samples (§3.3, Fig. 2).
- **Finetuning (HotpotQA, Fig. 3):** with 3,000 examples, finetuned ReAct becomes best of the four methods — finetuned PaLM-8B ReAct beats *all* PaLM-62B prompting, and finetuned PaLM-62B ReAct beats *all* 540B prompting (§3.3).

HotpotQA success/failure mode analysis (Table 2; 50 correct + 50 incorrect per method, 200 total):

| Mode | ReAct | CoT |
|------|-------|-----|
| True positive (success) | 94% | 86% |
| False positive (success) | 6% | 14% |
| Reasoning error (failure) | 47% | 16% |
| Search result error (failure) | 23% | — |
| Hallucination (failure) | 0% | 56% |
| Label ambiguity (failure) | 29% | 28% |

ALFWorld task-specific success rates % (Table 3, 134 unseen games, greedy decoding):

| Method | Pick | Clean | Heat | Cool | Look | Pick 2 | All |
|--------|------|-------|------|------|------|--------|-----|
| Act (best of 6) | 88 | 42 | 74 | 67 | 72 | 41 | 45 |
| ReAct (avg) | 65 | 39 | 83 | 76 | 55 | 24 | 57 |
| ReAct (best of 6) | 92 | 58 | 96 | 86 | 78 | 41 | 71 |
| ReAct-IM (avg) | 55 | 59 | 60 | 55 | 23 | 24 | 48 |
| ReAct-IM (best of 6) | 62 | 68 | 87 | 57 | 39 | 33 | 53 |
| BUTLERg (best of 8) | 33 | 26 | 70 | 76 | 17 | 12 | 22 |
| BUTLER (best of 8) | 46 | 39 | 74 | 100 | 22 | 24 | 37 |

- Best ReAct **71%** vs best Act **45%** vs best BUTLER **37%**; even the *worst* ReAct trial (**48%**) beats the best of both. ReAct beats Act across all six controlled trials, relative gain **33%–90%**, averaging **62%**. ReAct **71** vs ReAct-IM **53** overall (§4).

WebShop (Table 4, 500 test instructions):

| Method | Score | Success Rate |
|--------|-------|--------------|
| Act | 62.3 | 30.1 |
| ReAct | 66.6 | 40.0 |
| IL | 59.9 | 29.1 |
| IL+RL | 62.4 | 28.7 |
| Human Expert | 82.1 | 59.6 |

- ReAct gives an **absolute +10%** over the previous best success rate; one-shot Act already matches IL/IL+RL. All methods remain far below human experts (§4).
- **Appendix A.1 [unverified in main text]:** the paper states GPT-3 (text-davinci-002, greedy) *outperforms* PaLM-540B on HotpotQA and ALFWorld (Table 5); exact numbers are in the appendix, not reproduced here.

## Limitations
- **Prompt-context bottleneck.** Complex tasks with large action spaces need more demonstrations to learn well, which can exceed the in-context length limit — the core constraint of the prompting setup (§6, §1 contribution 4).
- **Reasoning rigidity / loops.** Interleaving thought-action-observation reduces flexibility, giving ReAct a *higher reasoning-error rate than CoT* (47% vs 16% of failures); a frequent ReAct-specific failure is repeating prior thoughts/actions and failing to break the loop (suspected greedy-decoding artifact) (§3.3, Table 2).
- **Retrieval dependence.** Non-informative search accounts for 23% of ReAct's HotpotQA errors and can derail reasoning — an expected factuality-vs-flexibility trade-off (§3.3).
- **Still far below supervised/human performance.** Prompting trails supervised SoTA on HotpotQA/FEVER (Table 1) and ReAct trails human experts on WebShop (§3.3, §4).
- **Closed, sanitized environments.** Experiments are limited to Wikipedia/WebShop to avoid private info or harmful actions; deploying LLM agents against open environments carries real risks (Ethics Statement).
- **Base-model accessibility.** Main results use PaLM-540B, which was not openly accessible at publication; GPT-3 results are provided for reproducibility (Reproducibility Statement).

## My Notes & Questions
- This is the foundational **agent-loop** paper — the thought→action→observation trajectory is the template behind most modern tool-using agents and frameworks; essential background for [[Agentic AI]] and [[Tool Calling]].
- The most transferable engineering lesson: **grounding beats memorization.** Finetuning Standard/CoT teaches a model to recall (possibly hallucinated) facts, while finetuning Act/ReAct teaches a *generalizable skill* (how to retrieve) — relevant to building retrieval-grounded agents over private corpora.
- The CoT-SC↔ReAct back-off is an early, pragmatic **router** between internal and external knowledge — conceptually similar to today's "decide whether to call a tool" logic.
- Open question for my SLM work: ReAct prompting is *worst* of the four at 8B/62B scale and only wins after finetuning — so for small models, **ReAct likely needs finetuning, not just prompting**. How few high-quality trajectories are actually needed at ~7B?
- Caveat worth remembering: the headline +34%/+10% gains are vs. *imitation/RL baselines*, and ReAct uses "best of 6" trial selection on ALFWorld — the **average** ReAct (57%) is the more honest single-run number.

## Review
2026-06-26 — VERDICT: PASS. Independently checked against arXiv 2210.03629 / ICLR 2023 metadata; benchmark numbers, setup details, limitations, wikilinks, and MOC coverage are consistent with the source and vault conventions.

## Related
- [[Agentic AI]]
- [[Chain-of-Thought]]
- [[Tool Calling]]
- [[Self-Consistency]]
- [[PaLM]]
- [[GPT-3]]
- [[HotpotQA]]
- [[FEVER]]
- [[ALFWorld]]
- [[WebShop]]
- [[Let's Verify Step by Step]]
