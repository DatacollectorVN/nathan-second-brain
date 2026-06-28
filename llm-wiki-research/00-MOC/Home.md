# LLM Wiki Research — Home

## 🗺️ Maps of Content
- [[Papers-MOC]] — All research papers
- [[Concepts-MOC]] — Core concepts & theory
- [[Experiments-MOC]] — My experiments & benchmarks
- [[Critical-Reviews-MOC]] — Comparative reviews & judgment notes

## 🔍 Research Focus Areas
- Small Language Models (SLMs)
- Agentic Systems & Tool Use
- LLM Memory & Knowledge Bases
- On-device / Edge Inference
- RLHF & Alignment

## 📥 Inbox
Drop raw notes and links in [[99-Inbox]]. Process weekly.

## 📊 Stats
```dataview
TABLE length(rows) AS "Count"
FROM "second-brain/llm-wiki-research"
WHERE file.name != "Home"
GROUP BY split(file.folder, "/")[2]
```
