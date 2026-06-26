---
title: LLM-to-SLM Agent Conversion
tags: [experiment, agentic, SLM, efficiency]
status: planned
hypothesis: At least one frequently-called subtask in an existing LLM-based agent can be replaced by a fine-tuned <10B SLM with no meaningful quality loss and a large cost/latency reduction.
date: 2026-06-16
---

## Goal
> What am I trying to find out?

Reproduce the 6-step conversion algorithm (S1–S6) from [[Small Language Models are the Future of Agentic AI]] on a small, real agent and measure how much of its LLM usage can be shifted to a specialized SLM. Concretely: can a fine-tuned SLM take over the highest-volume subtask (e.g. tool-call formatting or intent classification) while preserving end-to-end task success?

## Setup
- **Agent**: a simple existing tool-using agent (e.g. a coding or retrieval agent) currently backed by a frontier LLM API.
- **Teacher LLM**: the current generalist model (for distillation targets and as baseline).
- **Candidate SLMs**: [[Llama-3.1-8B-Instruct]], [[Qwen2.5-7B-Instruct]], and a smaller option ([[SmolLM2]] 1.7B or [[Hymba]] 1.5B) for the narrowest subtask.
- **Hardware**: single GPU (24–48GB) — sufficient given [[LoRA]]/QLoRA fine-tuning.
- **Framework**: PEFT ([[LoRA]]/QLoRA), plus an instrumentation/logging layer on the agent's model+tool interface.

## Method
> Step-by-step, mirroring S1–S6.

1. **S1 — Instrument & log** all non-HCI model/tool calls (prompt, output, tool-call payload, latency). Encrypted store, anonymized.
2. **S2 — Curate & filter** to ~10k–100k examples; strip PII/PHI; paraphrase sensitive entities.
3. **S3 — Cluster** prompts/actions to find recurring subtasks; pick 1–2 highest-volume clusters (e.g. JSON tool-call formatting, intent recognition).
4. **S4 — Select SLM** per chosen subtask by capability + footprint.
5. **S5 — Fine-tune** the SLM (PEFT; optionally distill from the teacher LLM on the same inputs). Enforce a **single tool-call format** (argument A5).
6. **S6 — Evaluate & iterate**: swap the SLM into the agent behind a router; measure quality and cost; retrain as needed.

## Results
| Metric | LLM baseline | SLM specialist | Notes |
|--------|--------------|----------------|-------|
| Subtask accuracy / format-validity rate | | | |
| End-to-end task success | | | |
| Latency per call | | | |
| Cost per call ($ / FLOPs) | | | |
| % of calls served by SLM | | | |

## Observations
- (to fill in)

## Conclusion
> What did I learn? Does it confirm the hypothesis?

(to fill in)

## Next Steps
- Add a second specialist and a router → move toward a full [[Heterogeneous Agentic Systems|heterogeneous system]].
- Test format-locking (A5) as an isolated intervention: does enforcing JSON-only output cut tool-call failures even before fine-tuning?
- Compare PEFT vs full fine-tuning vs distillation on the same subtask.

## Related
- [[Small Language Models are the Future of Agentic AI]]
- [[Agentic AI]]
- [[Heterogeneous Agentic Systems]]
- [[LoRA]]
