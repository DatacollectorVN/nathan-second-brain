# nathan-second-brain

A personal **knowledge graph for AI agents** — an Obsidian vault that captures all of Nathan's research, coursework, and study notes in a structured, linkable form that agents can read, write, and reason over.

Raw PDFs, lecture decks, and papers enter through inbox capture; agents process them into atomic notes linked by `[[wikilinks]]`. Obsidian's graph view makes the connections visible; Claude (via MCP) operates on the vault as a persistent memory layer for research and learning.

---

## What this is

| Layer | Role |
|-------|------|
| **Knowledge graph** | `[[wikilinks]]` between atomic notes (concepts, papers, models, experiments) form a navigable graph — not a flat folder of summaries |
| **Agent memory** | Each wiki ships `agents/worker.md`, `agents/reviewer.md`, and `agents/critical-reviewer.md` — reusable role prompts for a three-stage ingestion pipeline |
| **Human + agent co-authorship** | Nathan captures and curates; agents draft, cross-link, and verify; the `status` frontmatter field (`draft` → `needs-review` → `reviewed`) is the handoff baton |

The goal is not just storage. It is a **living second brain** that grows with every paper read, every lecture processed, and every experiment run — and that agents can query, extend, and audit without starting from scratch each session.

---

## Domain wikis

The vault is split into **domain wikis**, each following the same folder taxonomy but tuned to its subject matter. Start in the wiki that matches your task; each has its own detailed README.

```
nathan-second-brain/
├── llm-wiki-research/     → LLMs, SLMs, agentic systems, alignment, memory
├── ml-unlearning/         → Machine unlearning, differential privacy, certified deletion
├── rmit-courses/          → RMIT Master of AI coursework (lectures, labs, exams)
├── ml-research/           → Reserved for future ML research domains
├── How-Second-Brain-Work/ → Architecture notes & diagrams
└── scripts/               → Automation helpers (future)
```

| Wiki | Focus | Primary source layer |
|------|-------|----------------------|
| [`llm-wiki-research/`](llm-wiki-research/README.md) | Small language models, agentic AI, model routing, CoT/reasoning, RLHF/DPO | `01-Papers/` |
| [`ml-unlearning/`](ml-unlearning/README.md) | Exact & certified unlearning, LLM knowledge removal, privacy guarantees | `01-Papers/` |
| [`rmit-courses/`](rmit-courses/README.md) | RMIT Master of AI — lectures, readings, labs, past exams | `01-Documents/` |
| `ml-research/` | Placeholder for additional research tracks | — |

Concepts that span domains (e.g. **Attention**, **RLHF**, **Differential Privacy**) can exist in multiple wikis and cross-link as the graph grows.

---

## Shared folder pattern

Every active wiki uses the same **numbered-folder taxonomy**. Numbers sort folders in pipeline order; `99-Inbox` sinks to the bottom.

```
<wiki>/
├── 00-MOC/                  → Maps of Content — Dataview dashboards; start at Home.md
├── 01-Papers/ or 01-Documents/  → Primary sources (papers vs. course material)
├── 02-Concepts/             → Atomic idea notes — the graph's nodes
├── 03-Architectures/        → Specific models, methods, or systems
├── 04-Experiments/          → Lab notebook, reproductions, assignments
├── 05-Glossary/             → 2–3 sentence quick definitions
├── 06-Critical-Reviews/     → Comparative synthesis across sources
├── agents/                  → Worker / Reviewer / Critical Reviewer prompts
└── 99-Inbox/                → Raw capture, processed weekly
```

`rmit-courses/` adds `07-References/` (raw PDF intake log) and uses a **hybrid layout**: course-specific material (`01-Documents/`, `04-Experiments/`, `07-References/`) is split per course, while the knowledge-graph layer (`02-Concepts/`, `03-Architectures/`, `05-Glossary/`, `06-Critical-Reviews/`) stays **shared across all courses**.

---

## How the graph connects

```
99-Inbox (raw capture)
    ↓  weekly processing
Primary sources ──wikilink──► 02-Concepts (atomic ideas)
    │                              │
    └──── references ─────────► 03-Architectures (models & methods)
                                     │
04-Experiments ─── tests ────────────┘
                                     │
05-Glossary ◄── defines terms ───────┘

Reviewed sources ──────────────► 06-Critical-Reviews (synthesis & judgment)
```

**Wikilinks are the edges.** A paper note links to `[[Differential Privacy]]`; a lecture links to `[[Monte Carlo Simulation]]`; an experiment links to the architecture it tests. Obsidian builds the graph automatically — open **Graph View** (Cmd+G) to see the landscape.

MOCs (`00-MOC/`) are index nodes: Dataview queries aggregate notes by tag, status, and folder so agents and humans can navigate without memorising paths.

---

## Agent pipeline

Each wiki defines three agent roles in `agents/`:

| Agent | Job |
|-------|-----|
| **Worker** | Ingest a source (PDF, paper, lecture deck) → fill the template → create/update concept & architecture stubs → set `status: needs-review` |
| **Reviewer** | Independently verify the Worker's output against the source in a **fresh chat** → `reviewed` or `in-progress` + fix list |
| **Critical Reviewer** | After sources pass review, compare methods/models across notes → write opinionated synthesis in `06-Critical-Reviews/` |

Typical flow for new material:

1. **Chat 1 — Worker:** `Use agents/worker.md. Process the attached PDF.`
2. **Chat 2 (new) — Reviewer:** `Use agents/reviewer.md. Review <path> against the source.`
3. **Optional — Critical Reviewer:** compare related notes and record tradeoffs.

> Always run the Reviewer in a new chat. Shared context with the Worker defeats the quality gate.

See each wiki's README for domain-specific prompts (course resolution for RMIT, arXiv processing for research wikis, Google Drive archival for coursework).

---

## Current research & study areas

**LLM Wiki** — SLMs, agentic systems (ReAct, tool use, multi-agent), model routing & cascading, efficient reasoning (CoT, process/outcome supervision), alignment (RLHF, DPO, GRPO).

**ML Unlearning** — exact unlearning (SISA), certified approximate deletion ((ε,δ) guarantees), differential privacy, LLM knowledge unlearning, verification & membership inference.

**RMIT Courses** — Master of Artificial Intelligence coursework: AI foundations, Monte Carlo simulation, probability & statistics, search & rational agents, and more as semesters progress.

---

## Stack

- **Obsidian** — editor, graph view, wikilinks
- **Local REST API** plugin — exposes the vault to agents over HTTPS
- **Claude Desktop** — reads/writes notes via MCP
- **Google Drive** (coursework) — PDF archive, connected via MCP
- **Templater** — note templates with consistent frontmatter
- **Dataview** — query notes like a database (powers MOCs)
- **Smart Connections** — semantic search across notes (research wikis)
- **Obsidian Git** — sync to GitHub

---

## Quick start

1. Open the relevant wiki's `00-MOC/Home.md` in Obsidian.
2. Drop raw material in `99-Inbox/` or attach a PDF to a Claude chat.
3. Invoke the Worker: `Use agents/worker.md. Process the attached <paper|PDF>.`
4. In a **new chat**, run the Reviewer on the output.
5. Expand stubs, cross-link concepts, refresh MOCs as the graph grows.

For full prompt libraries, tag conventions, and weekly workflows, see the per-wiki READMEs linked above.

---

*Maintained by Nathan. RMIT Master of Artificial Intelligence.*
