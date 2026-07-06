---
title: Membership Inference
tags: [concept, verification, privacy]
status: draft
related_papers:
  - A Comprehensive Guide to Differential Privacy
date_added: 2026-07-06
---

## Definition
> One-sentence definition.

A membership inference attack (MIA) tries to determine whether a specific individual's data was included in a dataset or used to train a model.

## Intuition
> Explain it simply, as if to a colleague.

If a model behaves noticeably differently on data it saw during training versus fresh data, an attacker can exploit that gap to guess membership — directly violating the deniability DP aims to provide.

## How It Works
> Technical detail, math if applicable.

Early example: Homer et al. (2008) inferred whether an individual contributed DNA to a genomic mixture via statistical distance measures. Later demonstrated against aggregate location data and deep-learning models (Shokri et al. 2017). DP bounds MIA success because $(\varepsilon,\delta)$-indistinguishability limits how much the output can depend on any one record.

## Variants & Evolution
> How has this concept evolved across papers/models?

From genomics → aggregate statistics → deep models. In unlearning it is a primary **verification / auditing** tool: an MIA that still detects "forgotten" data indicates incomplete deletion.

## Key Papers
- [[A Comprehensive Guide to Differential Privacy]]

## Related Concepts
- [[Differential Privacy]]

## My Notes
MIA is the natural evaluation harness for unlearning — run it against the forget set before and after removal to test whether deletion actually took.
