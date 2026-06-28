---
title: RouterBench
tags: [glossary]
---

## Definition
**RouterBench** (Hu et al., 2024) — a widely-used benchmark for evaluating LLM routing methods, providing over 405k precomputed inference outputs from eleven diverse LLMs across seven tasks (MMLU, MT-Bench, MBPP, HellaSwag, WinoGrande, [[GSM8K]], ARC), with performance and cost metadata.

## Context
Because outputs and costs are precomputed, RouterBench lets researchers evaluate routing strategies offline under realistic cost constraints without re-running inference. Used as a standard testbed in [[Dynamic Model Routing and Cascading for Efficient LLM Inference - A Survey]] (e.g. for the Dueling Feedback bandit router). Related routing benchmarks: RouterEval, MixInstruct, LLMRouterBench.

## See Also
- [[Dynamic Model Routing and Cascading for Efficient LLM Inference - A Survey]]
- [[GSM8K]]
