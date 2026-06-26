---
title: Tool Calling
tags: [glossary, agentic]
---

## Definition
The ability of a language model to emit a structured, strictly-formatted request (e.g. JSON) that an agent's code parses and executes against an external tool or API, then feeds the result back to the model.

## Context
A core agent-relevant skill. [[Small Language Models are the Future of Agentic AI]] argues SLMs excel here precisely because the task is narrow and format-constrained (argument A5: agents need close behavioral alignment, so a format-locked SLM beats a hallucination-prone generalist). Models like xLAM-2-8B reach SOTA tool calling at small scale.

## See Also
- [[Agentic AI]]
- [[Heterogeneous Agentic Systems]]
