---
title: PATE
family: Teacher-Student DP
tags: [architecture, privacy]
status: draft
year: 2017
params:
context_length:
date_added: 2026-07-06
---

## Overview
> What is this architecture and why does it matter?

Private Aggregation of Teacher Ensembles (PATE), developed by Papernot et al. (2017, 2018), achieves DP by training an ensemble of "teacher" models on disjoint partitions of private data, then transferring knowledge to a "student" model via noisy aggregated voting on public/unlabeled data. Attractive when public data and disjoint training subsets are available.

## Key Design Choices
- Disjoint data partitions → independent teachers (parallel-composition style isolation).
- Noisy aggregation of teacher votes provides $(\varepsilon,\delta)$-DP.
- Only the student is released; teachers (and the private data) stay hidden.

## Comparison to Previous
| Feature | PATE | DP-SGD |
|---------|------|--------|
| Mechanism | Noisy teacher-vote aggregation | Gradient clipping + noise |
| Data assumption | Needs public/unlabeled data | None |
| Best regime | Disjoint subsets + public data | General deep training |

## Training Details
- Dataset: private data partitioned across teachers + public data for the student.
- Compute: trains many teacher models.
- Techniques: noisy argmax / Gaussian aggregation over teacher votes.

## Strengths & Weaknesses
**Strengths:** intuitive privacy via aggregation, strong when public data exists, model-agnostic teachers.
**Weaknesses:** requires public/unlabeled data and disjoint partitions; ensemble training overhead.

## Key Papers
- [[A Comprehensive Guide to Differential Privacy]]

## Related
- [[DP-SGD]]
- [[Composition]]
