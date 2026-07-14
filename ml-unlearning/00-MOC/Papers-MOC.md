# Papers — Map of Content

```dataview
TABLE source, tags, status
FROM "second-brain/ml-unlearning/01-Papers"
WHERE file.name != "_template"
SORT file.mtime DESC
```

## All Papers (static index)
<!-- Add papers here as they are processed, newest first:
- [[Paper Title]] — one-line description (year) · `status`
-->

- [[Machine Unlearning (SISA)]] — SISA training: sharded, isolated, sliced, aggregated exact unlearning via partial retraining from checkpoints (2021) · `needs-review`
- [[A Survey of Machine Unlearning]] — comprehensive survey: unlearning framework, exact/approximate definitions, taxonomy (model-agnostic / model-intrinsic / data-driven), verification, resources (2024) · `needs-review`
- [[Certified Data Removal from Machine Learning Models]] — (ε,δ)-certified removal via Newton-step deletion + loss perturbation on strongly convex linear models (2020) · `reviewed`
- [[Understanding Black-box Predictions via Influence Functions]] — influence functions for data attribution, efficient HVP-based computation, training-set attacks; foundation of influence-based unlearning (2017) · `reviewed`
- [[A Comprehensive Guide to Differential Privacy]] — survey of DP theory, mechanisms, DP-SGD/accounting, and deployment (2025) · `reviewed`
