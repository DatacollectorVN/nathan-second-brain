---
title: Outcome Supervision
tags: [concept, reasoning, alignment]
status: draft
related_papers:
  - "[[Let's Verify Step by Step]]"
date_added: 2026-06-26
---

## Definition
Outcome supervision trains a [[Reward Model]] (an [[ORM]]) using only the **final result** of a solution — typically whether the final answer is correct — with no feedback on the intermediate steps.

## Intuition
It is the cheap, automatable option: when answers are auto-checkable (as in [[MATH]]), no humans are needed. But it forces the reward model to solve a hard credit-assignment problem — given only "right/wrong at the end," it must infer *where* a wrong solution failed — and it rewards **false positives**: solutions that reach the correct answer through incorrect reasoning.

## How It Works
An [[ORM]] is trained to predict whether a complete solution is correct, with the score read from the prediction at the final token. Correctness labels usually come from automatic final-answer checking. Contrast with [[Process Supervision]], which labels each step.


*An ORM ignores the steps and scores only the final result:*

```mermaid
flowchart LR
    S1["Step 1"] --> S2["Step 2"] --> S3["Step 3"] --> F["Final answer"]
    F --> R["ORM score: correct or not (final token only)"]
```

## Variants & Evolution
Standard approach in verifier work (e.g. Cobbe et al. 2021). [[Let's Verify Step by Step]] (2023) used it as the baseline that [[Process Supervision]] beat, and distinguished outcome supervision *by final-answer checking* from outcome supervision *by a large PRM* (the latter avoids some false positives).

## Key Papers
- [[Let's Verify Step by Step]]

## Related Concepts
- [[Process Supervision]]
- [[Reward Model]]
- [[RLHF]]

## My Notes
Stub — created from [[Let's Verify Step by Step]]. The false-positive weakness is arguably over-emphasized by MATH's auto-checkable answers.
