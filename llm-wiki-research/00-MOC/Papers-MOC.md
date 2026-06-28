# Papers — Map of Content

```dataview
TABLE source, tags, status
FROM "second-brain/llm-wiki-research/01-Papers"
WHERE file.name != "_template"
SORT file.mtime DESC
```

## All Papers (static index)
- [[Top AI Agentic Workflow Patterns]] — qualitative overview of five core agentic design patterns (Reflection, Tool Use, ReAct, Planning, Multi-Agent Collaboration) (2025) · `needs-review`
- [[ReAct - Synergizing Reasoning and Acting in Language Models]] — interleave reasoning traces + actions; LLM agents on HotpotQA/FEVER/ALFWorld/WebShop (2023) · `reviewed`
- [[Let's Verify Step by Step]] — process vs outcome supervision; PRMs, PRM800K, MATH (2023) · `reviewed`
- [[Small Language Models are the Future of Agentic AI]]
- [[Efficient Long CoT Reasoning in Small Language Models]]
- [[Attention Is All You Need]]
- [[Dynamic Model Routing and Cascading for Efficient LLM Inference - A Survey]] — survey of inference-time routing/cascading across independently trained LLMs; six paradigms + a three-axis (when/what/how) design-space framework (2026) · `needs-review`
