# Critical Reviews — Map of Content

```dataview
TABLE status, covers, tags, file.mtime AS "Updated"
FROM "second-brain/llm-wiki-research/06-Critical-Reviews"
WHERE file.name != "_template"
SORT file.mtime DESC
```

## Published Reviews

- [[Agentic Workflow Patterns — Comparative Review]] — `draft` — compares Reflection / Tool Use / ReAct / Planning / Multi-Agent patterns; covers [[Top AI Agentic Workflow Patterns]]. (Underlying pattern notes are still `draft` and the source blog is `needs-review` — see the review's Evidence Base.)

## Review Queue

- Add reviewed papers, methods, or architectures here when they need a higher-level comparison or judgment note.
