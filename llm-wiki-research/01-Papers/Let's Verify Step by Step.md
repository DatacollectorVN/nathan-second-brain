---
title: Let's Verify Step by Step
authors: Hunter Lightman, Vineet Kosaraju, Yura Burda, Harri Edwards, Bowen Baker, Teddy Lee, Jan Leike, John Schulman, Ilya Sutskever, Karl Cobbe
year: 2023
arxiv: https://arxiv.org/abs/2305.20050
tags:
  - paper
  - reasoning
  - alignment
status: reviewed
topic:
  - Process Supervision
  - Reward Modeling
  - Mathematical Reasoning
  - LLM Alignment
related_concepts:
  - "[[Process Supervision]]"
  - "[[Outcome Supervision]]"
  - "[[Reward Model]]"
  - "[[Chain-of-Thought]]"
  - "[[RLHF]]"
date_added: 2026-06-26
---

## Summary
Even state-of-the-art LLMs regularly make logical mistakes during multi-step reasoning, and a single bad step can derail an entire solution. A common fix is to train a [[Reward Model]] that scores candidate solutions and is then used for search (best-of-N / rejection sampling) or [[RLHF|RL]] — but the resulting system is only as reliable as the reward model. This paper compares two ways of training that reward model: **[[Outcome Supervision]]** (feedback on the final answer only, an [[ORM]]) versus **[[Process Supervision]]** (feedback on every intermediate reasoning step, a [[PRM]]). On the challenging [[MATH]] dataset, process supervision **significantly outperforms** outcome supervision: the best PRM solves **78.2%** of a representative 500-problem subset of the MATH test set via best-of-1860 search. The authors also show a large PRM can stand in for human labelers to drive cheap ablations, that **active learning** yields a **2.6×** data-efficiency gain, and they release **[[PRM800K]]**, the full dataset of 800,000 step-level human labels.

## Key Contributions
- **Process supervision trains far more reliable reward models than outcome supervision.** Their state-of-the-art PRM solves **78.2%** of a representative subset of the MATH test set (best-of-1860), versus 72.4% (ORM) and 69.6% (majority voting) (Fig. 3, §3).
- **Large-RM-as-supervisor (synthetic supervision).** A large PRM (`PRMlarge`) can reliably approximate human supervision for smaller reward models, enabling large-scale data-collection ablations that would be infeasible with human labelers (§4).
- **Active learning improves data efficiency ~2.6×** for process supervision by surfacing "convincing wrong-answer" solutions for labeling (§4.2).
- **Open data release: [[PRM800K]]** — 800K step-level human feedback labels across 75K solutions to 12K problems, used to train the best PRM (§2.4).

## Architecture / Method

### Setup and evaluation protocol
- At each scale a single fixed model — the **generator** — produces all candidate solutions; it is *not* improved with RL. "Outcome vs process supervision" refers strictly to the supervision given to the **reward model**, not to the generator (§2.1).
- The generator is fine-tuned to emit solutions in a **newline-delimited, step-by-step format** (few-shot generate → filter to correct final answers → fine-tune 1 epoch), purely to make per-step parsing easy, not to teach new skills (§2.3).
- Reward models are evaluated by **best-of-N search**: for each test problem, sample N solutions uniformly from the generator, pick the one the RM ranks highest, auto-grade its final answer, and report the fraction correct (§2.1).

### Base models and MathMix
- Large-scale models are fine-tuned from the **base GPT-4** (next-token pretraining only, *no* RLHF). Small-scale base models share GPT-4's design but used **~200× less pretraining compute** (§2.2).
- All models get a lightweight pretraining stage on **MathMix**, ~**1.5B** math-relevant tokens (vs Minerva's 38.5B), more aggressively filtered to math problem-solving content and with no explicitly mixed-in general language data (§2.2, App. A).

### Outcome-supervised Reward Model (ORM)
- Trained (following Cobbe et al. 2021) to predict whether a full solution is correct/incorrect; correctness is normally determined by **automatic final-answer checking**. The score is the ORM's prediction at the **final token** (§2.5).
- Weakness baked in: final-answer grading gives **false positives** — solutions that reach the right answer via wrong reasoning are mislabeled positive (§2.5, §4).

### Process-supervised Reward Model (PRM)
- Predicts the correctness of **each step**, as a single token emitted after that step's last token; training maximizes the log-likelihood of these target tokens, so it fits a standard LM pipeline with one forward pass at test time (§2.6).
- **Solution score = probability that every step is correct**, implemented as the **product of per-step correctness probabilities** (§2.6).
- Supervision deliberately stops at the **first incorrect step**, which keeps the ORM-vs-PRM comparison clean and keeps human labeling cost comparable (identifying the first mistake ≈ judging overall correctness) (§2.6).
- Human labels are **positive / negative / neutral**; "neutral" defers ambiguity so it can be treated as positive or negative at test time (§2.4).

### Data collection & active learning
- Human data-labelers grade each step of generator-sampled MATH solutions (interface in Fig. 1). To maximize value of the limited labeling budget, the system **surfaces "convincing wrong-answer" solutions** — rated highly by the current best PRM yet reaching a wrong final answer — since the PRM is provably wrong about ≥1 step (§2.4).
- The PRM is **iteratively retrained** during collection; at each iteration generate N solutions/problem and surface the top-K most convincing wrong-answer ones (top-K applied per-problem or globally) (§2.4).
- **Active-learning recipe (§4.2):** train a small `PRMselector` on 1 sample/problem → score 1000 samples/problem → select N/problem as **80% most-convincing wrong-answer + 20% most-convincing remaining** → score selected samples with `PRMlarge` and train on those scores. Estimated **~2.6× more data-efficient** than uniform labeling.

### Small-scale synthetic supervision (§4)
- To isolate two confounders in the large-scale comparison — (1) the ORM and PRM training sets aren't directly comparable, and (2) final-answer grading's false positives — they use `PRMlarge` to supervise smaller models, simulating large-scale collection cheaply. This enables a direct comparison of process supervision, outcome supervision (by `PRMlarge`), and outcome supervision (by final-answer checking) on otherwise-identical datasets.

## Results & Benchmarks

Main comparison on the representative **500-problem subset of the MATH test set** (best-of-1860 search, Fig. 3, §3):

| Benchmark                                            | Score | Baseline              |
| ---------------------------------------------------- | ----- | --------------------- |
| MATH subset — PRM (process-supervised), best-of-1860 | 78.2% | —                     |
| MATH subset — ORM (outcome-supervised), best-of-1860 | 72.4% | Majority voting 69.6% |
| MATH subset — Majority voting, best-of-1860          | 69.6% | —                     |

Out-of-distribution generalization on fresh STEM exams (best-of-100, 100 test samples/problem, Table 1, §5):

| Exam (OOD) | ORM | PRM | Majority Vote | # Problems |
|------------|-----|-----|---------------|------------|
| AP Calculus | 68.9% | 86.7% | 80.0% | 45 |
| AP Chemistry | 68.9% | 80.0% | 71.7% | 60 |
| AP Physics | 77.8% | 86.7% | 82.2% | 45 |
| AMC10/12 | 49.1% | 53.2% | 32.8% | 84 |
| **Aggregate** | **63.8%** | **72.9%** | **61.3%** | **234** |

Other quantitative claims from the paper:
- **Active learning ≈ 2.6×** data efficiency vs uniform labeling (§4.2).
- In the small-scale study, **process supervision beats both forms of outcome supervision at all data-collection scales** (best-of-500, Fig. 4a); outcome supervision *by `PRMlarge`* beats outcome supervision *by final-answer checking* (Fig. 4b, §4.1).
- **PRM800K:** 800K step-level labels / 75K solutions / 12K problems; 4.5K MATH test problems are folded into training so evaluation is on the remaining **500** test problems (§2.4).
- The PRM's advantage over both baselines **widens as N grows** (Fig. 3, §3).

> [unverified] §5 prose states the OOD set is "**224** STEM questions," but Table 1's per-exam counts sum to **234** (45+60+45+84) and the aggregate row also reads 234. The paper is internally inconsistent here; the table value (234) is reproduced above. Flag for the Reviewer.

## Limitations
- **Math-only evidence.** All results are in mathematical reasoning; the authors explicitly note it is unknown how broadly process supervision generalizes to other domains (§6.2, §8).
- **Non-comparable large-scale training sets.** The large-scale ORM and PRM training sets are not apples-to-apples — the PRM set is built via active learning, biased toward wrong-answer solutions, and an order of magnitude smaller. The small-scale synthetic study (§4) exists specifically to work around this, but the headline large-scale comparison is caveated (§3, §4).
- **Possible MATH test-set contamination.** Some MATH problems likely appear in pretraining data; string-matching removal can't catch rephrasings. The authors saw no clear memorization and argue any contamination should affect methods similarly, but cannot rule it out (§6.3).
- **Active-learning instability.** Iteratively retraining `PRMselector` during collection produced unexplained instability and no improvement over the static scheme; the benefit of iterative retraining remains unproven (§4.2).
- **Diversity ceiling on active learning.** The largest active-learning model (200 samples/problem) slightly underperforms the trend line, attributed to 200 being a large fraction of the 1000-sample selection pool, limiting diversity (§4.2).
- **Scope excludes RL-tuning the generator.** Fine-tuning the generator with RL from the reward model — the natural next step — is intentionally out of scope (§2.1).

## My Notes & Questions
- This is the canonical **PRM / process-reward** paper and the foundation for later verifier-driven test-time-compute work; useful background context for [[Overthinking in Reasoning Models]] and reasoning-focused SLMs.
- The **"convincing wrong-answer" selection rule** is a cheap, reusable data-curation pattern worth stealing for any labeling pipeline: spend annotation budget where the current model is *confidently wrong*, not on uniformly sampled (often obviously-bad) outputs.
- **Scoring caveat:** defining a solution's score as the *product* of per-step correctness probabilities is mathematically clean but compounds toward zero for long solutions; Appendix F discusses alternatives. Relevant if reusing PRMs on long agentic traces.
- The **"negative alignment tax"** framing is the notable alignment angle: the safer, more interpretable method is also the more capable one — the opposite of the usual safety/performance trade-off.
- Open question for my SLM work: the small-scale comparisons lean on `PRMlarge` (a GPT-4-grade model) as the supervision oracle rather than humans. Does process supervision still pay off at ~7B scale **without** a frontier-grade generator/labeler in the loop?
- **[[PRM800K]]** is openly released and directly usable — a candidate dataset for experiments in `04-Experiments/`.

## Related
- [[Process Supervision]]
- [[Outcome Supervision]]
- [[Reward Model]]
- [[Chain-of-Thought]]
- [[RLHF]]
- [[PRM]]
- [[ORM]]
- [[PRM800K]]
- [[MATH]]
- [[GSM8K]]
- [[Best-of-N]]
- [[Active Learning]]

---

## Review
**2026-06-26 — VERDICT: PASS** (independently verified against arXiv 2305.20050v1, re-extracted from source PDF)

| # | Check | Result |
| --- | --- | --- |
| 1 | Faithfulness | ✅ All numbers trace to source: 78.2/72.4/69.6 confirmed as **Best-of-1860** figures (p.7 Fig. 3; also Table 4); full OOD Table 1 exact incl. row order & 234 count, at best-of-100 (§5, p.10); PRM800K 800K/75K/12K (§2.4), 4.5K→500 test split (§2.4 & App. C), ~200× compute & MathMix 1.5B vs Minerva 38.5B (§2.2, App. A) all verified |
| 2 | Completeness | ✅ Every template section filled, no leftover placeholders/blockquotes; frontmatter complete and sensible |
| 3 | Wikilinks | ✅ All 13 links resolve to real notes/created stubs. ⚠️ Non-blocking: `[[RLHF]]` resolves to the Glossary note rather than the richer `02-Concepts/RLHF.md` (pre-existing concept/glossary duplicate in vault, not introduced here); `best-of-N` in Summary prose left unlinked though `[[Best-of-N]]` exists |
| 4 | Conventions | ✅ Tags (`paper`, `reasoning`, `alignment`) from README vocabulary; correct folder. Stub `dataset` tag follows existing de-facto pattern (cf. `MATH` → `benchmark`) |
| 5 | Cross-note | ✅ All 8 stubs exist (Process/Outcome Supervision, Reward Model; PRM, ORM, PRM800K, Best-of-N, Active Learning); Papers-MOC.md updated (static index + Dataview) |
| 6 | Calibration | ✅ Limitations honest and traceable; My Notes clearly separated as opinion; the 224-vs-234 inconsistency was caught and flagged; depth appropriate for a Lead Data Engineer |

Optional non-blocking follow-ups: pipe `[[RLHF]]` to the concept note (e.g. `[[RLHF|RLHF]]` is already glossary; use path-qualified link if the concept note is intended), and wikilink "best-of-N" on first mention in the Summary.
