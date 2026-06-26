---
title: Small Language Models (SLMs)
tags: [concept, models, efficiency, on-device]
status: stable
related_papers:
  - "[[Efficient Long CoT Reasoning in Small Language Models]]"
  - "[[Small Language Models are the Future of Agentic AI]]"
date_added: 2025-06-13
---

## Definition
Small Language Models (SLMs) are language models with significantly fewer parameters than frontier LLMs — typically in the 1B–13B range — designed for efficiency, on-device deployment, and specialized tasks.

## Intuition
Frontier models (GPT-4, Claude, DeepSeek-R1) are 100B–700B+ parameters — too big to run on a laptop or phone. SLMs are the "right-sized" models that you can actually deploy, fine-tune, and own. The 7B class (Llama-3-8B, Qwen-2.5-7B, Mistral-7B) has become the de facto research sweet spot.

> A useful task-oriented definition: [[Small Language Models are the Future of Agentic AI]] defines an SLM as a model that **fits on a common consumer device and serves one user's agentic requests at practical latency** — comfortably under ~10B params as of 2025.

## Why SLMs Matter
- **Cost** — orders of magnitude cheaper inference
- **Latency** — faster generation
- **Privacy** — runs locally, no data leaves device
- **Customization** — feasible to fine-tune on your own hardware
- **Edge deployment** — phones, IoT, embedded systems

## Common SLM Sizes
| Tier | Params | Examples | Hardware |
|------|--------|----------|----------|
| Tiny | <1B | TinyLlama, SmolLM-360M | Phone CPU |
| Small | 1-3B | Phi-3-mini, Gemma-2B | Phone / laptop |
| Medium | 7-9B | Llama-3.1-8B, Qwen-2.5-7B, Mistral-7B | Single GPU |
| Large SLM | 13-32B | Mistral-Nemo, Phi-4, QwQ-32B | High-end GPU |

## Key Challenges
- **Capability gap** — fundamental limits compared to frontier models
- **Reasoning** — struggle with complex multi-step problems
- **Long context** — quadratic attention costs hurt more
- **Distillation difficulty** — see [[Efficient Long CoT Reasoning in Small Language Models]]: SLMs have "relatively poor capacity and generalization" for absorbing complex teacher traces

## Strategies to Boost SLM Capability
1. **Better data** — heavily curated training corpora (Phi approach)
2. **Distillation from larger models** — see [[Knowledge Distillation]]
3. **Test-time compute** — let SLMs "think longer" via CoT
4. **Tool use & agentic patterns** — augment capability with external tools
5. **Specialization** — narrow scope for higher quality in domain

## SLMs in Agentic Systems
[[Small Language Models are the Future of Agentic AI]] (Belcak et al., NVIDIA, 2025) argues SLMs should be the **default** model in agents, because agent calls are mostly narrow, repetitive, scoped, and non-conversational. Headline claims: SLMs already match LLMs on agent-relevant skills (tool calling, code-gen, instruction following), and a 7B SLM is **10–30× cheaper** to serve than a 70–175B LLM. Where general conversation is needed, use [[Heterogeneous Agentic Systems]]. See also the [[LLM-to-SLM Agent Conversion]] playbook.

## Key Papers
- Phi-3 Technical Report (Abdin et al., 2024)
- Llama-3 Technical Report (Meta, 2024)
- Qwen2.5 Technical Report (Qwen Team, 2024)
- [[Efficient Long CoT Reasoning in Small Language Models]]
- [[Small Language Models are the Future of Agentic AI]]

## Related Concepts
- [[Knowledge Distillation]]
- [[Chain-of-Thought]]
- [[On-device Inference]]
- [[Agentic AI]]
- [[Heterogeneous Agentic Systems]]

## My Notes
This is **the core focus area** of my RMIT research. The interesting frontier: how do we make 7B-class models punch above their weight via better training data, smarter distillation, and agentic patterns? The Efficient Long CoT paper is a great example — it's not about making models bigger, it's about making *training data* SLM-aware. The NVIDIA position paper zooms out: re-architect the whole *agent* around SLMs, not just optimize one model.
