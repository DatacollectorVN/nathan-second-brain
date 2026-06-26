---
title: Small Language Models are the Future of Agentic AI
authors: Peter Belcak, Greg Heinrich, Shizhe Diao, Yonggan Fu, Xin Dong, Saurav Muralidharan, Yingyan Celine Lin, Pavlo Molchanov
year: 2025
arxiv: https://arxiv.org/abs/2506.02153
tags:
  - paper
  - SLM
  - agentic
  - efficiency
  - position-paper
status: reviewed
topic:
  - Small Language Models
  - Agentic AI
  - Heterogeneous Agentic Systems
  - Inference Economics
related_concepts:
  - "[[Small Language Models]]"
  - "[[Agentic AI]]"
  - "[[Heterogeneous Agentic Systems]]"
  - "[[Tool Calling]]"
  - "[[Knowledge Distillation]]"
date_added: 2026-06-16
---

## Summary
A **position paper** from NVIDIA Research (and Georgia Tech) arguing that small language models (SLMs), not large ones, are the right default for most of what agentic systems actually do. The authors observe that agents typically invoke a model to perform a *small number of specialized, repetitive, narrowly-scoped, non-conversational subtasks* (intent parsing, tool-call formatting, summarization, code emission) — exactly the kind of work where a well-designed SLM already matches frontier LLM quality at a fraction of the cost. They formulate the stance as a value statement (V1–V3: SLMs are sufficiently powerful, more operationally suitable, and more economical), support it with arguments A1–A13, rebut the main counter-views (AV1–AV3), diagnose the practical barriers to adoption (B1–B3), and provide a concrete 6-step **LLM-to-SLM agent conversion algorithm** (S1–S6). Where general conversation is genuinely needed, they advocate **heterogeneous agentic systems** that default to SLMs and call LLMs selectively.

---

## Key Contributions
- A clearly-framed **position** that SLMs (working definition: fits on a consumer device and serves one user's agentic requests at practical latency; comfortably <10B params as of 2025) are the future of agentic AI, expressed as value statements V1–V3.
- A capability survey showing modern SLMs already rival much larger models on the *agent-relevant* skills: commonsense reasoning, tool calling, code generation, and instruction following (Phi, Nemotron-H, SmolLM2, Hymba, DeepSeek-R1-Distill, RETRO, xLAM-2, Toolformer).
- An economic argument (A2): serving a 7B SLM is **10–30× cheaper** than a 70–175B LLM in latency/energy/FLOPs, with cheaper fine-tuning (a few GPU-hours), edge deployability, and better parameter-utilization.
- The **heterogeneous / "Lego-like" agent** thesis: compose small specialized experts, invoke an LLM only when general reasoning or open-domain dialogue is essential.
- A practical, deployable **LLM→SLM conversion algorithm** (S1–S6) grounded in the natural data-collection structure of agents.
- Honest treatment of the opposing case: alternative views AV1–AV3 and barriers B1–B3 are stated and addressed rather than dismissed.

---

## Architecture / Method
This is a position paper, so the "method" is its argumentative structure plus the conversion algorithm.

### Working definitions
- **WD1 — SLM**: a LM that fits on a common consumer electronic device and runs inference at latency low enough to be practical when serving one user's agentic requests.
- **WD2 — LLM**: any LM that is not a SLM.
- As of 2025 the authors treat most models **below ~10B parameters** as SLMs.

### The position (V1–V3)
- **V1** — SLMs are *principally sufficiently powerful* for the language-modeling errands of agentic applications.
- **V2** — SLMs are *inherently more operationally suitable* for agentic systems than LLMs.
- **V3** — SLMs are *necessarily more economical* for the vast majority of in-agent LM uses.

### Supporting arguments (selected)
- **A1** SLMs are already powerful enough (steeper size→capability curve; competitive on agent-relevant benchmarks).
- **A2** SLMs are more economical (10–30× cheaper inference; cheap PEFT; edge deployment; sparser parameter use).
- **A3** SLMs are more flexible (cheap to train many specialized experts → democratization).
- **A4** Agents expose only a *narrow* slice of LM functionality (a fine-tuned SLM suffices for the chosen prompts).
- **A5** Agentic interactions need *close behavioral alignment* — strict, single-format tool calls/outputs favor a format-locked SLM over a hallucination-prone generalist.
- **A6** Agentic systems are *naturally heterogeneous* — a model can be a tool of another model; mix sizes per subtask.
- **A7** Agentic interactions are a *natural data source* — a logger on the tool/model interface harvests specialist fine-tuning data.

### Two modes of agency (Figure 1)
- **Language-model agency** (left): the LM is both the human-computer interface (HCI) and the orchestrator of tool calls.
- **Code agency** (right): a dedicated controller code orchestrates everything; the LM (optionally) fills the HCI role — here *every* LM can in principle be a specialized SLM.

### LLM-to-SLM agent conversion algorithm (S1–S6)
1. **S1 Secure usage-data collection** — instrument the agent to log all non-HCI calls (prompts, outputs, tool-call contents, latency) via encrypted pipelines with role-based access and anonymization.
2. **S2 Data curation & filtering** — collect ~10k–100k examples; strip PII/PHI/sensitive data; paraphrase to obfuscate entities.
3. **S3 Task clustering** — unsupervised clustering of prompts/actions to surface recurring tasks (intent recognition, extraction, summarization, code-gen) as SLM specialization candidates.
4. **S4 SLM selection** — per task, pick candidate SLMs by capability, benchmark fit, license, and deployment footprint.
5. **S5 Specialized fine-tuning** — build task datasets; fine-tune via PEFT ([[LoRA]]/QLoRA) or full FT; optionally distill from the LLM ([[Knowledge Distillation]]).
6. **S6 Iteration & refinement** — periodically retrain SLMs and the router on fresh data; a continuous improvement loop back to S2/S4.

---

## Results & Benchmarks
> Position paper — these are illustrative capability/efficiency claims drawn from cited SLMs, not new experiments.

| Model (size) | Claim | Reference comparison |
|--------------|-------|----------------------|
| Phi-2 (2.7B) | Commonsense + code on par with 30B, ~15× faster | 30B same-gen |
| Phi-3-small (7B) | Language understanding/commonsense on par with, code near, 70B | 70B same-gen |
| Nemotron-H (2/4.8/9B, hybrid Mamba-Transformer) | Instruction-following + code-gen ≈ dense 30B at ~10× fewer FLOPs | dense 30B |
| SmolLM2 (125M–1.7B) | Matches 14B contemporaries; rivals 70B from 2 yrs prior | 14B / 70B |
| Hymba-1.5B | Best-in-class instruction accuracy; 3.5× throughput; beats 13B | 13B |
| DeepSeek-R1-Distill-Qwen-7B | Outperforms Claude-3.5-Sonnet-1022 and GPT-4o-0513 (reasoning) | proprietary LLMs |
| RETRO (7.5B) | Comparable to GPT-3 (175B) on LM with 25× fewer params | GPT-3 175B |
| xLAM-2-8B | SOTA tool calling; surpasses GPT-4o and Claude 3.5 | frontier LLMs |
| Toolformer (6.7B) | Outperforms GPT-3 (175B) via learned API use | GPT-3 175B |

Headline economic claim: a **7B SLM is 10–30× cheaper** to serve than a 70–175B LLM in latency, energy, and FLOPs; SLM fine-tuning takes only a few GPU-hours.

---

## Limitations
- It is a **position paper**, not an empirical study — no new experiments; evidence is a curated reading of others' results.
- The SLM/LLM boundary is **deliberately fuzzy** (device-relative, ~10B in 2025), so claims shift as hardware improves.
- The economics of AV2 (LLM centralization / economy of scale) are conceded to be **case-specific** — "the jury is still out."
- Real obstacles remain: utilizing/load-balancing many specialist SLM endpoints (CA3) and infra setup + talent costs (CA4).
- Scope limited to text LMs (vision-language models noted as a natural extension but not treated).

---

## My Notes & Questions
- This is the natural "macro" companion to [[Efficient Long CoT Reasoning in Small Language Models]]: that paper makes a single SLM cheaper *per task*; this one argues the whole *agent* should be re-architected around SLMs. Strong fit for my RMIT SLM-for-agents direction.
- **A5 (behavioral alignment)** is the most actionable for engineering: locking a single tool-call format (JSON only) and fine-tuning an SLM to never deviate could cut a lot of agent flakiness. Worth a small experiment.
- **A7 + S1** (the logger that harvests specialist data) is essentially a free distillation flywheel — the agent generates its own future training set. The privacy/anonymization step (S2) is the hard part in practice.
- Open question: how do you build and cheaply *route* across many specialist SLMs without the router itself becoming a bottleneck or a generalist LLM in disguise? (This is the unaddressed gap behind CA3.)
- The Nemotron-H / Hymba results are NVIDIA's own models — worth reading their primary papers before taking the FLOP claims at face value.

---

## Related
- [[Small Language Models]]
- [[Agentic AI]]
- [[Heterogeneous Agentic Systems]]
- [[Tool Calling]]
- [[Knowledge Distillation]]
- [[Nemotron-H]]
- [[Hymba]]
- [[SmolLM2]]
- [[Phi]]
- [[DeepSeek-R1]]
- [[LLM-to-SLM Agent Conversion]]
- [[Efficient Long CoT Reasoning in Small Language Models]]

---

## Review
**2026-06-17 — VERDICT: PASS** (verified against arXiv 2506.02153 v2)

| # | Check | Result |
|---|-------|--------|
| 1 | Faithfulness | ✅ Entire capability table exact (Phi-2 2.7B ~15×; Phi-3-small 7B; Nemotron-H order-of-magnitude FLOPs; SmolLM2; Hymba-1.5B 3.5×; DeepSeek-R1-Distill-Qwen-7B vs Claude-3.5-Sonnet-1022/GPT-4o-0513; RETRO-7.5B 25× fewer params; xLAM-2-8B; Toolformer-6.7B). 10–30× / 70–175B economic claim, V1–V3, WD1/WD2, A1–A7 labels all correct |
| 2 | Completeness | ✅ after fix — removed two leftover template placeholder blockquotes in Summary and Architecture/Method (cleaned during this review) |
| 3 | Wikilinks | ✅ All resolve |
| 4 | Conventions | ✅ Tags valid (`position-paper` a reasonable addition) |
| 5 | Cross-note | ✅ Auto-indexed by Papers-MOC Dataview |
| 6 | Calibration | ✅ Limitations match paper's own caveats; My Notes clearly personal |

Note: S1–S6 step details and the "~10k–100k examples" figure (Section 6) were not independently re-verified this pass — consistent with the paper's stated 6-step algorithm, nothing contradicts them. Status left at `reviewed`.
