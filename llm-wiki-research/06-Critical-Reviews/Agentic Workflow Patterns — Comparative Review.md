---
title: "Agentic Workflow Patterns — Comparative Review"
type: critical-review
status: draft
date_added: 2026-06-27
reviewer: Nathan
covers:
  - "[[Top AI Agentic Workflow Patterns]]"
  - "[[Reflection Pattern]]"
  - "[[Tool Use Pattern]]"
  - "[[ReAct Pattern]]"
  - "[[Planning Pattern]]"
  - "[[Multi-Agent Pattern]]"
tags:
  - critical-review
---

## Question / Thesis

When should an agent reach for each of the five canonical agentic workflow patterns — [[Reflection Pattern]], [[Tool Use Pattern]], [[ReAct Pattern]], [[Planning Pattern]], [[Multi-Agent Pattern]] — and which are actually distinct design primitives versus composable layers of the same loop? This review compares them on thesis, assumptions, mechanism, training data, evaluation, benchmark meaning, strengths, failure modes, deployment fit, and research implications, and stakes out an opinionated default for production [[Agentic AI|agentic systems]].

> **Confidence note.** The single source that treats these five as a coordinated set — [[Top AI Agentic Workflow Patterns]] — is a `needs-review` secondary blog post with **no benchmarks**, and all five pattern notes are `status: draft`. The only `reviewed` quantitative evidence I draw on concerns *specific instantiations* ([[ReAct - Synergizing Reasoning and Acting in Language Models|ReAct]], process-supervised verification in [[Let's Verify Step by Step]], and the SLM economics in [[Small Language Models are the Future of Agentic AI]]) — not the generic patterns. Treat pattern-level numbers as illustrative, not as head-to-head measurements. See Evidence Base.

## Compared Items

- [[Reflection Pattern]]
- [[Tool Use Pattern]]
- [[ReAct Pattern]]
- [[Planning Pattern]]
- [[Multi-Agent Pattern]]

## Executive Judgment

These are **not five competing architectures**; they are four orthogonal capabilities plus one composition strategy, and most are layers you stack rather than alternatives you pick. [[Tool Use Pattern|Tool use]] is the foundational primitive (an agent that cannot act is just a chatbot); [[ReAct Pattern|ReAct]] is the minimal control loop that makes tool use *reliable* by interleaving reasoning with action; [[Reflection Pattern|reflection]] and [[Planning Pattern|planning]] are quality/latency trade-offs bolted onto that loop (critique-then-revise, or decompose-then-execute); and the [[Multi-Agent Pattern|multi-agent pattern]] is an orchestration layer that distributes the other four across specialized roles. My default recommendation: **start with tool use inside a ReAct loop, add reflection only where correctness dominates latency, add explicit planning only in predictable/expensive-to-backtrack domains, and reach for multi-agent last** — its coordination and debugging cost is real and, per [[Top AI Agentic Workflow Patterns]], it is the hardest to test. The strongest *grounded* evidence in the vault is that the ReAct loop measurably beats blind acting and reduces hallucination ([[ReAct - Synergizing Reasoning and Acting in Language Models|ReAct]] paper), and that the high-value parts of these loops — formatted tool calls, narrow critiques — are exactly the workloads that [[Small Language Models|SLMs]] handle cheaply ([[Small Language Models are the Future of Agentic AI]]).

## Evidence Base

**Source notes reviewed (and their confidence):**

- [[Top AI Agentic Workflow Patterns]] — `status: needs-review`. **Lower confidence.** Secondary source (ByteByteGo blog), qualitative only, no benchmarks. This is the *only* note covering all five patterns together, so the comparative framing leans on it heavily — flagged accordingly.
- [[Reflection Pattern]], [[Tool Use Pattern]], [[ReAct Pattern]], [[Planning Pattern]], [[Multi-Agent Pattern]] — all `status: draft`. **Lower confidence**; none has passed a source-faithfulness review. Used for mechanism/variant descriptions, not for numerical claims.

**Reviewed papers used to ground specific claims (higher confidence):**

- [[ReAct - Synergizing Reasoning and Acting in Language Models]] — `reviewed` (PASS, verified vs arXiv 2210.03629). Grounds all ReAct-pattern numbers below.
- [[Let's Verify Step by Step]] — `reviewed` (PASS, verified vs arXiv 2305.20050). Grounds the *critic/verifier* angle of the Reflection pattern (process- vs outcome-supervised reward models).
- [[Small Language Models are the Future of Agentic AI]] — `reviewed` (PASS, verified vs arXiv 2506.02153). Grounds deployment-fit and economics takeaways.

**Supporting concept/glossary notes:** [[Agentic AI]] (`in-progress`), [[Heterogeneous Agentic Systems]] (`in-progress`), [[Chain-of-Thought]] (`stable`), [[Tool Calling]] (glossary stub), [[Self-Consistency]].

**Experiments / implementation evidence:** [[LLM-to-SLM Agent Conversion]] (`planned`, not yet run) — relevant to the "distill the loop into SLMs" takeaway but provides no results yet.

**Key benchmark anchors that exist in source notes (with caveats):**

- ReAct vs Act vs imitation baseline on ALFWorld: best ReAct **71%** vs best Act **45%** vs best BUTLER **37%**; honest single-run average ReAct **57%** (per the [[ReAct - Synergizing Reasoning and Acting in Language Models|ReAct]] note, which flags "best-of-6" selection). WebShop success rate: ReAct **40.0%** vs IL+RL **28.7%** vs human **59.6%**.
- ReAct vs CoT failure modes on HotpotQA: hallucination-caused failures **0% (ReAct) vs 56% (CoT)**, but reasoning-error failures **47% (ReAct) vs 16% (CoT)** — grounding cuts fabrication but rigidifies reasoning.
- Verifier evidence relevant to Reflection's critic-generator variant: process-supervised reward model **78.2%** vs outcome-supervised **72.4%** vs majority vote **69.6%** on the MATH subset ([[Let's Verify Step by Step]]). **Apples-to-oranges caveat:** this is a *trained external verifier on math*, not an LLM critiquing its own free-form draft — it is suggestive of "separate critic > self-grading," not proof of the generic Reflection loop.
- SLM economics: a 7B SLM is **10–30× cheaper** to serve than a 70–175B LLM, and small models already hit SOTA tool calling (xLAM-2-8B) ([[Small Language Models are the Future of Agentic AI]]).

## What Each Does Well

- **[[Tool Use Pattern]]** — *Paper claim:* lets the model offload math, fresh facts, and DB/API access to dedicated systems, with the LLM dynamically choosing when/how to call and recovering from errors. *Grounded:* tool calling is the one agent skill where [[Small Language Models|SLMs]] are already competitive with frontier models ([[Tool Calling]]; [[Small Language Models are the Future of Agentic AI]]), making it the cheapest layer to specialize.
- **[[ReAct Pattern]]** — *Grounded (reviewed):* interleaving thought→action→observation beats blind acting on every controlled ALFWorld trial and nearly eliminates hallucination on HotpotQA (0% vs 56% of failures) while making the trajectory interpretable/debuggable. Best general-purpose default loop.
- **[[Reflection Pattern]]** — *Paper claim:* iterative self-critique raises quality on code and technical writing. *Grounded analog:* a *separate* critic/verifier outperforms self-grading and majority voting in the math setting of [[Let's Verify Step by Step]], supporting the note's "critic-generator (multi-model)" variant over single-model self-critique.
- **[[Planning Pattern]]** — *Paper claim:* upfront decomposition + dependency mapping reduces backtracking and makes scheduling predictable; dynamic replanning recovers from step failures. Best where steps are expensive or parallelizable.
- **[[Multi-Agent Pattern]]** — *Paper claim:* role specialization (researcher/coder/critic/coordinator) beats forcing one generalist to juggle competing constraints, with cleaner separation of concerns; maps naturally onto [[Heterogeneous Agentic Systems]] (big model orchestrates, SLMs execute).

## Failure Modes / Risks

- **[[Tool Use Pattern]]** — Tool selection and error handling *become part of the agent's failure surface* (timeouts, schema-invalid args, bad retrievals). The [[ReAct - Synergizing Reasoning and Acting in Language Models|ReAct]] note quantifies the retrieval risk: non-informative search caused **23%** of ReAct's HotpotQA errors.
- **[[ReAct Pattern]]** — *Grounded:* higher reasoning-error rate than CoT (**47% vs 16%** of failures) and a characteristic **hallucination/repetition loop** where the model repeats a thought-action and cannot break out. Interpretability does not equal correctness.
- **[[Reflection Pattern]]** — Latency and token cost from sequential LLM calls (per the note's own "My Notes"); self-critique can rubber-stamp its own errors (the false-positive problem [[Let's Verify Step by Step]] documents for outcome-only grading); product-of-step scoring compounds toward zero on long traces.
- **[[Planning Pattern]]** — Planning overhead is *wasted* under high uncertainty; static plans fail if any step fails; per [[Top AI Agentic Workflow Patterns]], formal upfront planning gives little benefit on simple linear tasks.
- **[[Multi-Agent Pattern]]** — Coordination/communication overhead, requires explicit protocols, and is the hardest to debug because failures emerge from *interactions* between agents. (This is also the user's standing open question in [[Top AI Agentic Workflow Patterns]]: how to test for regressions when one agent's prompt/model changes.)

## Tradeoff Matrix

| Item | Strength | Weakness | Best Fit | Avoid When |
|------|----------|----------|----------|------------|
| [[Tool Use Pattern]] | Grounds model in real systems; cheapest layer to put on an SLM | Adds tool-failure surface; ~23% of ReAct errors were bad retrievals | Any task needing fresh facts, math, code exec, or APIs | Pure closed-book generation where no external state matters |
| [[ReAct Pattern]] | Beats blind acting on every ALFWorld trial; ~0% hallucination failures; interpretable | Higher reasoning-error rate (47% vs CoT 16%); repetition loops | Default loop for dynamic, partially-observable tasks | Simple linear tasks; when verbose traces cost too much |
| [[Reflection Pattern]] | Lifts quality on code/writing; separate critic > self-grading (78.2% vs 72.4% verifier analog) | Latency × token cost from extra passes; self-critique can rubber-stamp errors | High-stakes correctness: code, audits, technical copy | Latency-critical paths; simple factual lookups |
| [[Planning Pattern]] | Cuts backtracking; enables parallelism + predictable scheduling | Overhead wasted under uncertainty; brittle if static | Predictable, multi-step, expensive-to-backtrack pipelines | Short/linear tasks or fast-changing environments |
| [[Multi-Agent Pattern]] | Specialization + separation of concerns; maps onto SLM-heavy heterogeneous systems | Coordination overhead; hardest to debug/regression-test | Large workflows with genuinely distinct roles | When a single ReAct agent already suffices |

## Engineering Takeaways

- **Stack, don't choose.** Treat tool use + ReAct as the base loop; add reflection and planning as opt-in modules gated by the task's correctness/latency profile. Adopting all five at once buys cost and debugging pain you rarely need.
- **Make reflection a *separate* critic, not self-grading.** The verifier evidence in [[Let's Verify Step by Step]] (process-supervised PRM 78.2% vs outcome/self 72.4%) argues for an independent critique signal — ideally a cheap specialized [[Small Language Models|SLM]] critic — over asking the generator to grade itself.
- **Budget for ReAct's loop pathology.** Add explicit loop-breakers (step caps, repetition detection, a CoT-SC/self-consistency back-off as in the ReAct paper's router) before shipping a ReAct agent.
- **Distill the high-frequency layers into SLMs.** Tool-call formatting and narrow critiques are format-constrained, repetitive subtasks — exactly the [[Small Language Models are the Future of Agentic AI|SLM]] sweet spot (10–30× cheaper). This is the hypothesis already queued in [[LLM-to-SLM Agent Conversion]].
- **Prefer code agency for control flow.** Keep deterministic orchestration in code and call the model only for structured decisions (the user's own note in [[Top AI Agentic Workflow Patterns]]); multi-agent "choreography" should still sit on a deterministic backbone for testability.

## Research Implications

- **No common benchmark exists for the patterns as a family.** ReAct has rigorous evals; reflection/planning/multi-agent are described qualitatively in the source. A genuinely useful contribution would be a *single task suite* measuring all five (and their compositions) on success rate, latency, and token cost.
- **Multi-agent regression testing is open.** Cascading effects when one agent's prompt/model changes (the standing open question in [[Top AI Agentic Workflow Patterns]]) have no accepted methodology — a concrete research target.
- **Reflection at small scale is unproven.** [[Let's Verify Step by Step]] relies on a frontier-grade verifier; whether a ~7B self-critic adds value without a large oracle in the loop is untested (mirrors the open question in the ReAct note about finetuning ReAct at 7B).
- **Routing is the bottleneck for multi-agent/heterogeneous systems.** Per [[Heterogeneous Agentic Systems]], a cheap-yet-accurate router is unsolved; without it, the multi-agent pattern's specialization savings evaporate.

## My View

*Explicitly opinionated; separated from the paper-authored claims above.*

I think the framing of "five patterns" in [[Top AI Agentic Workflow Patterns]] is pedagogically useful but **conceptually misleading as a menu**. Only two of the five are real primitives — tool use (the ability to act) and ReAct (the loop that makes acting reliable). Reflection and planning are *policies over that loop* (when to re-examine output, when to commit to a plan), and multi-agent is an *org chart* imposed on top. Presenting them as peers invites teams to "adopt the multi-agent pattern" as if it were a feature, when in practice it is a tax you pay only when roles are genuinely separable.

My strongest opinion: **reflection is over-recommended and multi-agent is over-adopted relative to their grounded evidence.** The cleanest quantitative support in the vault is for ReAct (reviewed paper, real baselines) and for *external verification* (Let's Verify) — and the latter is evidence for a separate critic, not the popular self-reflection loop. I would bet that a single well-instrumented ReAct agent with a cheap SLM critic and hard loop-breakers beats a four-role multi-agent system on most production tasks, at a fraction of the debugging cost. This is a falsifiable bet — exactly what [[LLM-to-SLM Agent Conversion]] could start to test.

Where I am least confident: the planning vs ReAct boundary. The source's claim that planning wins in "predictable" domains and ReAct in "uncertain" ones is intuitive but, in the vault, **entirely unbenchmarked** — I am repeating the blog, not endorsing a measured result.

## Open Questions

- Is iterative *self*-reflection (single model) ever better than a separate cheap critic, or does the [[Let's Verify Step by Step]] result generalize: always prefer an independent verifier?
- At what task complexity does the multi-agent pattern's specialization benefit actually exceed its coordination + regression-testing cost? Is there a measurable crossover?
- Can ReAct's repetition-loop failure be fixed structurally (state-tracking, loop detection) rather than by sampling tricks?
- How few high-quality trajectories does a ~7B model need to run a reliable ReAct loop (open question carried from the [[ReAct - Synergizing Reasoning and Acting in Language Models|ReAct]] note)?
- What does a cheap, accurate router look like for default-SLM/escalate-to-LLM agents ([[Heterogeneous Agentic Systems]]) without the router becoming a generalist LLM in disguise?

## Related

- [[Top AI Agentic Workflow Patterns]]
- [[Agentic AI]]
- [[Heterogeneous Agentic Systems]]
- [[Small Language Models are the Future of Agentic AI]]
- [[ReAct - Synergizing Reasoning and Acting in Language Models]]
- [[Let's Verify Step by Step]]
- [[Chain-of-Thought]]
- [[Tool Calling]]
- [[LLM-to-SLM Agent Conversion]]
