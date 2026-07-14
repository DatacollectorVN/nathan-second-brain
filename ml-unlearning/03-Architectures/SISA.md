---
title: SISA (Sharded, Isolated, Sliced, Aggregated)
family: exact unlearning / efficient retraining
tags: [architecture, exact-unlearning]
status: needs-review
year: 2021
params:
context_length:
date_added: 2026-07-14
---

## Overview
> SISA training (Bourtoule et al., IEEE S&P 2021 — see [[Machine Unlearning (SISA)]]) is the canonical data-partitioning framework for efficient unlearning: it strategically limits each training point's influence *by construction* so that deletion requires retraining only a small piece of the ensemble, from a saved checkpoint, while providing the same distributional guarantee as retraining from scratch ([[Exact Unlearning]] w.r.t. the SISA training procedure).

## Key Design Choices
- **Sharding:** the dataset $D$ (size $N$) is uniformly partitioned into $S$ disjoint shards; one constituent model per shard; each point lives in exactly one shard (maximizes unlearning savings). Expected speed-up per request: $S\times$.
- **Isolation:** no gradients or updates flow between constituents. This structurally confines a point's influence to one model — the source of the provable, intuitive guarantee — at the cost of potential weak learners on small shards.
- **Slicing:** each shard is split into $R$ slices, trained incrementally ($D_{k,1}$, then $D_{k,1}\cup D_{k,2}$, ...) with a parameter checkpoint saved before each new slice; unlearning restarts from the last checkpoint that excludes the deleted point. Works for any stateful iterative learner (gradient descent — LR, SVM, DNNs); not for greedy decision-tree induction. Epochs must be recalibrated: $e' = \frac{2R}{R+1}e_0$, where $e_0$ = epochs without slicing — this keeps total training time equal because early steps see less data.
- **Aggregation:** label-based majority vote by default; must not involve training data (else the aggregator itself would need unlearning). Prediction-vector averaging recovers accuracy on complex tasks (+1.68 PPs top-1 / +4.37 PPs top-5 on ImageNet). Learned controllers were rejected — modest gains, extra unlearning liability.
- **Distribution-aware sharding (optional):** if the request distribution is known, sort points by erasure probability $p(u)$ and pack shards so each shard's expected number of requests $\mathbb{E}(\chi_i)$ (Poisson binomial) stays below a constant $C \le 1$ — likely-to-be-deleted data concentrates in few, small shards. Costs ~1 PP accuracy on SVHN (94.4% vs. 95.7% uniform).

## Comparison to Previous
| Feature | SISA | Naive retraining | [[Certified Removal]] (Guo et al.) |
|---------|------|------------------|------------------------------------|
| Guarantee | Exact (distributional, w.r.t. sharded procedure) | Exact (trivially) | $(\epsilon,\delta)$ probabilistic |
| Deletion cost (1 request) | Retrain 1 shard from checkpoint; best case $\frac{(R+1)S}{2}\times$ faster | Full retrain | Newton-step update |
| Model class | Any (slicing needs stateful iterative training) | Any | Effectively linear/convex |
| Accuracy | <2 PPs loss on simple tasks; larger on complex (mitigate w/ transfer learning) | Baseline | Perturbed objective cost |

## Training Details
- Datasets (from the paper): MNIST (60,000), Purchase (250,000; 600 features, 2 classes), SVHN (604,833), CIFAR-100 (60,000), Mini-ImageNet (128,545), ImageNet (1,281,167).
- Models: 2 FC (Purchase), 2 conv + 2 FC (MNIST), Wide ResNet-1-1 (SVHN), ResNet-50 (CIFAR-100/ImageNet/Mini-ImageNet); same architecture and hyperparameters for all constituents.
- Compute/storage trade: checkpoint storage linear in $R$; hyperparameter search is $O(R)$ under uniform sharding with shared hyperparameters (worst case $O(SR)$).
- Key expected-cost results (uniform requests, $K$ = requests): batched sharding $\mathbb{E}[C] = N(1-(1-\tfrac{1}{S})^K) - K$ — the expected fraction of shards hit governs cost; sequential slicing $\mathbb{E}[C] = e_0 D(\tfrac{2}{3} + \tfrac{1}{3R})$ with $D = N/S$ — max expected slicing speed-up 1.5× as $R \to \infty$.
- Measured: Purchase 4.63×, SVHN 2.45× (S=20, R=50, <2 PPs cost); ImageNet 1.36×; Mini-ImageNet 8.01× for 4 requests (−16.7 PPs); transfer learning ImageNet→CIFAR-100 at S=10 shrinks the gap to ~4 PPs top-1 / <1 PP top-5.
- Regime: speed-up while $K < 3S$ (≈ ≤0.075% of dataset for sharding, ≤0.003% for slicing at S=20); beyond it SISA gracefully degrades to full-retrain cost. Google-scale deletion rates (~$10^{-6}$ of the dataset) sit comfortably inside the regime. $S > 20$ costs >5 PPs accuracy even on simple tasks.
- Extensions (per [[A Survey of Machine Unlearning]]): shard graphs tracking update dependencies (SAFE, Dukler et al.), graph-aware partitioning (GraphEraser), SISA-inspired task isolation in lifelong learning; SNARK/Merkle proof-of-unlearning demonstrated on SISA.

## Strengths & Weaknesses
**Strengths:** exact distributional guarantee that is intelligible and auditable (the training algorithm is the certificate); model-agnostic; minimal pipeline changes; handles sequential and batched requests with analytical cost bounds; reference implementation at cleverhans-lab/machine-unlearning.
**Weaknesses:** speed-up bounded to the $K < 3S$ regime; weak-learner accuracy loss on complex tasks; checkpoint storage; needs stateful learners for slicing; aggregation constrained to training-data-free schemes; assumes honest provider (no third-party verification); assumes points appear in exactly one shard (replication/dedup breaks this); distribution-aware variant needs a known, stable request distribution and raises fairness concerns.

## Key Papers
- [[Machine Unlearning (SISA)]] — primary source (Bourtoule et al., 2021)
- [[A Survey of Machine Unlearning]] — situates SISA in the data-driven / efficient-retraining family

## Related
- [[Exact Unlearning]]
- [[Certified Removal]]
- [[Differential Privacy]]
- [[Influence Functions]]
