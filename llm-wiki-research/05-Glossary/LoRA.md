---
title: LoRA (Low-Rank Adaptation)
tags: [glossary, efficiency]
---

## Definition
A parameter-efficient fine-tuning (PEFT) method that freezes the base model and trains small low-rank adapter matrices added to its weights, drastically cutting the memory and compute needed to adapt a model.

## Context
Makes specializing [[Small Language Models|SLMs]] cheap — a key enabler in the [[LLM-to-SLM Agent Conversion]] pipeline (step S5) from [[Small Language Models are the Future of Agentic AI]]. Related variants: QLoRA (quantized base) and DoRA.

## See Also
- [[Knowledge Distillation]]
- [[Small Language Models]]
