# Machine Unlearning — Intensive Self-Study Roadmap

**Audience:** Strong ML/DL background, new to unlearning.
**Horizon:** ~2–4 weeks, intensive (several hours/day).
**Goal:** Reach *research-ready* — able to read any unlearning paper critically, reproduce a baseline, and identify open sub-problems.

> Note: This roadmap is compiled from web research, not the `llm-wiki-research` vault (it was unavailable at time of writing). Cross-check against your own notes and migrate the durable parts in.

---

## How to use this

Each phase has: **objective**, **core readings**, **what to extract**, and a **milestone** (something you produce, not just read). Don't move on until the milestone is done — passive reading is the trap here. Keep a running notes file with, per paper: the exact unlearning *definition* used, the *guarantee* claimed, the *assumptions*, and the *evaluation*. Those four fields are where all the disagreement in this field lives.

---

## Phase 0 — Prerequisite refresh (1–2 days, skip what you know)

You have the ML/DL. Unlearning leans on three things people often haven't touched:

- **Differential privacy (DP):** the (ε, δ) formalism. Certified unlearning borrows its indistinguishability definition directly. Read the intro + definitions of Dwork & Roth, *The Algorithmic Foundations of Differential Privacy* (Ch. 1–3 only).
- **Influence functions:** Koh & Liang, *Understanding Black-box Predictions via Influence Functions* (ICML 2017). This is the mathematical engine behind one-shot ("just update the weights") unlearning.
- **Convex optimization / strong convexity:** you need to *feel* why certified guarantees hold for convex losses and break for deep nets. Skim any convex-opt refresher on strong convexity and the Hessian.

**Milestone:** In your own words (½ page), state what "(ε, δ)-indistinguishable from retraining" means and why it's the natural definition of "forgotten."

---

## Phase 1 — Frame the problem (2–3 days)

**Objective:** Get the taxonomy and vocabulary so later papers slot into a mental grid.

Core readings:
- Nguyen et al., *A Survey of Machine Unlearning* (2022) — arXiv:2209.02299. Read fully; it's the map.
- Xu et al., *Machine Unlearning: A Survey* (ACM Computing Surveys, 2023) — arXiv:2306.03558. Read for the taxonomy (exact / approximate / certified; sample vs. class vs. feature).

What to extract: the axes of the field — **guarantee strength** (exact → certified → empirical), **granularity** (sample / class / concept), **efficiency vs. utility vs. guarantee** trade-off triangle, and **verification** (how you'd even check).

**Milestone:** Draw the taxonomy as a one-page diagram. Place every method name the surveys mention onto it. This diagram becomes your reference for the rest of the plan.

---

## Phase 2 — Exact unlearning (2 days)

**Objective:** Understand the "provably identical to retraining" end and why it's expensive.

Core reading:
- Bourtoule et al., *Machine Unlearning* (IEEE S&P 2021) — the **SISA** method (Sharded, Isolated, Sliced, Aggregated training).

What to extract: how sharding turns deletion into "retrain one shard," the storage/compute cost, and where exact unlearning stops scaling.

**Milestone:** Reproduce a toy SISA setup — train on a sharded dataset (e.g., CIFAR-10 or even MNIST), delete one point, retrain only its shard. Measure the wall-clock saving vs. full retrain. This gets your hands dirty early.

---

## Phase 3 — Certified / approximate unlearning (4–5 days, the core)

**Objective:** Master the theory that gives *provable* guarantees without full retraining. This is the heart of "certificated unlearning."

Core readings (in order):
1. Guo et al., *Certified Data Removal from Machine Learning Models* (2020) — the anchor. The (ε, δ) certified-removal framework via a Newton-step update + loss perturbation.
2. Sekhari et al., *Remember What You Want to Forget* (NeurIPS 2021) — generalization guarantees and **deletion capacity** (how many points you can remove before the guarantee dies).
3. Neel et al., *Descent-to-Delete* (2021) — gradient-based unlearning with **sequential** deletion guarantees.
4. Nguyen et al., *Variational Bayesian Unlearning* (2020) — the Bayesian/posterior view.

What to extract: for each — the update rule, the assumptions (convexity? bounded gradients?), what exactly is certified, and how the guarantee degrades over multiple deletions. **This is where you'll find your research gap** — every one of these is proven under assumptions deep nets violate.

**Milestone:** Implement Guo et al.'s certified removal on **logistic regression** (convex, so the theory actually holds). Verify empirically that the unlearned model ≈ retrained model. You now own the canonical certified method end-to-end.

---

## Phase 4 — The deep-net frontier (2 days)

**Objective:** See how (and how poorly) certificates extend past convexity.

Core reading:
- *Certified Unlearning for Neural Networks* (2025) — arXiv:2506.06985.
- Skim one federated/decentralized certified paper (e.g., arXiv:2601.06436) to see how the setting changes.

What to extract: which assumptions they relax, what they buy, and what they still can't guarantee. Note the open problems explicitly — these are candidate thesis topics.

**Milestone:** One page: "What breaks when you move certified unlearning from convex models to deep nets, and what have people tried." This is essentially a mini related-work section.

---

## Phase 5 — LLM / concept unlearning + evaluation (3–4 days)

**Objective:** Understand the most active sub-area and, crucially, why current evaluation is untrustworthy.

Core readings:
- Benchmarks: **TOFU** (fictitious authors), **WMDP** (hazardous bio/cyber knowledge), **MUSE** (news/books), **RWKU** (real-world individuals). Read each benchmark's paper intro + metrics.
- *LUME: LLM Unlearning with Multitask Evaluations* (2025) — arXiv:2502.15097.
- **Critical papers (read closely):**
  - *Position: LLM Unlearning Benchmarks are Weak Measures of Progress* (ICLR 2025) — arXiv:2410.02879.
  - *Does Machine Unlearning Truly Remove Knowledge?* (2025) — arXiv:2505.23270.
  - *Catastrophic Failure of LLM Unlearning via Quantization* (ICLR 2025) — knowledge resurfaces after quantizing.

What to extract: the difference between *suppressing* an output and *removing* the knowledge; the recovery attacks (relearning, quantization, prompting) that expose "fake" unlearning.

**Milestone:** Run one existing unlearning method on a small model against a TOFU-style forget set, then try to *recover* the "forgotten" info via a simple attack (e.g., a few fine-tuning steps or a paraphrased prompt). Seeing it come back is the single most clarifying experience in this field.

---

## Phase 6 — Synthesize into a research angle (2 days)

**Objective:** Turn study into a direction.

Do:
- Revisit your Phase-1 taxonomy diagram and mark, in red, every cell where the survey/critical papers say "open" or "fails."
- Write a 1–2 page gap analysis. The strongest current gaps: **(a)** certified guarantees for non-convex/deep models, **(b)** third-party *verifiable/auditable* deletion, **(c)** robustness — does it stay forgotten under recovery attacks, **(d)** sequential deletion at scale.

**Milestone:** A short problem statement: one gap, why it matters, why it's unsolved, and a first experiment you could run this week.

---

## Suggested pacing (2-week intensive)

| Days | Phase |
|------|-------|
| 1–2  | Phase 0 (prereqs) + start Phase 1 |
| 3–4  | Phase 1 (taxonomy) + Phase 2 (SISA) |
| 5–8  | Phase 3 (certified theory — the big one) |
| 9–10 | Phase 4 (deep-net frontier) |
| 11–13| Phase 5 (LLM unlearning + evaluation) |
| 14   | Phase 6 (synthesize gap + problem statement) |

Stretch to 3–4 weeks if you want to do every milestone thoroughly (recommended — the coding milestones are what make you research-ready rather than just well-read).

---

## Running-notes template (use for every paper)

```
Title / authors / year:
Unlearning definition used:
Guarantee claimed (exact / certified (ε,δ) / empirical):
Key assumptions:
Method in one sentence:
Evaluation (datasets, metrics, attacks):
Limitations / what's open:
How it relates to my gap:
```

---

## Core reading list (quick reference)

**Surveys:** Nguyen 2022 (2209.02299) · Xu 2023 (2306.03558) · Comprehensive Survey 2024 (2405.07406)
**Exact:** Bourtoule 2021 (SISA)
**Certified:** Guo 2020 · Sekhari 2021 · Neel 2021 (Descent-to-Delete) · Nguyen 2020 (Variational Bayesian)
**Deep-net frontier:** Certified Unlearning for NNs 2025 (2506.06985) · Federated 2601.06436
**LLM + eval:** TOFU · WMDP · MUSE · RWKU · LUME (2502.15097) · Weak-Benchmarks position (2410.02879) · Does Unlearning Truly Remove Knowledge (2505.23270) · Quantization failure (ICLR 2025)
