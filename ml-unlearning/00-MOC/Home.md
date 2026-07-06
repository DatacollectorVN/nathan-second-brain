# ML Unlearning Research — Home

## 🗺️ Maps of Content
- [[Papers-MOC]] — All research papers
- [[Concepts-MOC]] — Core concepts & theory
- [[Experiments-MOC]] — My experiments & benchmarks
- [[Critical-Reviews-MOC]] — Comparative reviews & judgment notes

## 🔍 Research Focus Areas
- Exact unlearning (SISA, sharded retraining)
- Certified / approximate unlearning ((ε,δ) guarantees)
- Unlearning for deep / non-convex models
- LLM knowledge & concept unlearning
- Verification, auditing & robustness of forgetting

## 📥 Inbox
Drop raw notes and links in [[99-Inbox]]. Process weekly.

## 📊 Stats
```dataview
TABLE length(rows) AS "Count"
FROM "second-brain/ml-unlearning"
WHERE file.name != "Home"
GROUP BY split(file.folder, "/")[2]
```
