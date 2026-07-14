---
title: Data Poisoning
tags: [concept, verification]
status: draft
related_papers:
  - "[[Understanding Black-box Predictions via Influence Functions]]"
date_added: 2026-07-13
---

## Definition
> Data poisoning (training-set attack) is the deliberate manipulation of training data — inserting, relabeling, or imperceptibly perturbing points — so that the trained model misbehaves on chosen test inputs.

## Intuition
The training-set analogue of adversarial examples: instead of perturbing the test input, perturb what the model learns from. Points the model is least confident about (ambiguous, mislabeled) carry the most influence, making them the most effective poisoning targets.

## How It Works
Koh & Liang (2017) craft poisons with influence functions: iterate $\tilde z_i \leftarrow \Pi\left(\tilde z_i + \alpha\,\text{sign}\!\left(\mathcal{I}_{\text{pert,loss}}(\tilde z_i, z_{\text{test}})\right)\right)$, retraining after each step.

**What each symbol means:** $\alpha$ is the step size (0.02 in the paper); $\Pi$ projects onto images sharing the original's 8-bit representation (visual indistinguishability); $\mathcal{I}_{\text{pert,loss}}$ is the influence of an input perturbation on the target test loss.
**Logic:** ascend the direction that maximally increases loss at $z_{\text{test}}$ while staying visually identical — mathematically equivalent to the KKT-derived attacks of Biggio et al. (2012) for continuous data. One perturbed training image flipped 57% of dog-vs-fish test predictions on an Inception-v3-based model.

## Variants & Evolution
- Biggio et al. (2012): SVM poisoning, visually distinguishable.
- Koh & Liang (2017): first visually-indistinguishable training attack on accurate neural nets.
- Relevance to unlearning: poisoned/highly-influential points are natural deletion targets, and unlearning is a candidate remediation for discovered poisoning.

## Key Papers
- [[Understanding Black-box Predictions via Influence Functions]]

## Related Concepts
- [[Influence Functions]]
- [[Membership Inference]]

## My Notes
Stub created by Worker while processing Koh & Liang 2017.
