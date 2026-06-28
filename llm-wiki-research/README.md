# 🧠 LLM Wiki Research

A personal research knowledge base on Large/Small Language Models, agentic systems, and LLM memory — built with Obsidian + Claude (via MCP).

---

## 📁 Folder Structure & Roles

```
llm-wiki-research/
├── 00-MOC/            → Maps of Content (index dashboards)
├── 01-Papers/         → Research papers — one note per paper
├── 02-Concepts/       → Atomic concept notes — the building blocks
├── 03-Architectures/  → Model families & specific models
├── 04-Experiments/    → My own experiments & lab notebook
├── 05-Glossary/       → Quick definitions (2-3 sentences)
├── 06-Critical-Reviews/ → Comparative reviews, tradeoffs, and judgment notes
├── agents/            → Role instructions for Worker / Reviewer / Critical Reviewer
└── 99-Inbox/          → Raw capture, processed weekly
```

### 00-MOC — Maps of Content
Dashboard notes that index everything via Data view queries. Start every research session by opening `Home.md`. Auto-updates as you add notes.

### 01-Papers — Research papers
One note per arXiv paper, blog post, or technical report. Each follows `_template.md`: summary, contributions, method, benchmarks, limitations, my notes. **This is the biggest folder.**

### 02-Concepts — Atomic ideas
Reusable building blocks that papers reference: Attention, RLHF, KV Cache, CoT, etc. One concept = one note. Papers link *into* this folder via `[[wikilinks]]`. Over time this becomes a personal LLM encyclopedia.

### 03-Architectures — Specific models
One note per model family or architecture: Transformer, Mamba, Llama-3, DeepSeek-R1. Different from concepts — this is about *specific models*, parameter counts, training details, design choices.

### 04-Experiments — Lab notebook
Hands-on work: fine-tuning runs, benchmarks, prompt experiments, paper reproductions. Tracks hypothesis → setup → results → conclusion. Even *planned* experiments live here.

### 05-Glossary — Quick reference
2-3 sentence definitions only. Link here when you need a fast reminder of what "perplexity" or "logit" means without reading a full concept note.

### 06-Critical-Reviews — Comparative judgment layer
Human + agent reviews of models, methods, and paper clusters. These notes are explicitly analytical: compare alternatives, surface tradeoffs, identify failure modes, and record your own engineering/research judgment. Use this folder after the source notes are already processed and, ideally, reviewed.

### agents — Agent role instructions
Reusable prompts for the three-agent workflow:
`agents/worker.md`, `agents/reviewer.md`, and `agents/critical-reviewer.md`.

### 99-Inbox — Raw capture
Paste links, paper titles, tweets, half-formed ideas here. Process into proper folders weekly. The `99` prefix pushes it to the bottom of the file tree.

---

## 🔗 How They Connect

```
99-Inbox (raw)
    ↓ process weekly
01-Papers ──links to──► 02-Concepts
    │                        │
    └──── references ─────► 03-Architectures
                                │
04-Experiments ─── tests ────► 03-Architectures
                                │
05-Glossary ◄── defines terms ─ everywhere

Reviewed source notes ───────► 06-Critical-Reviews
                              compares / critiques / synthesizes
```

The power is in `[[wikilinks]]`. Obsidian builds a knowledge graph automatically — open **Graph View** (Cmd+G) to see your research landscape visually.

---

## 🤖 Working with Claude (via MCP)

This vault is connected to Claude Desktop through the Obsidian Local REST API MCP server. Claude can read, create, and update notes directly. Below are battle-tested prompt patterns.

### 1. Process a Paper (most common)

**Just drop the PDF or paste the abstract + arXiv link.** Then say:

> Add this paper to my LLM wiki. Process it into `01-Papers/` using the template, and create or update related notes in `02-Concepts/`, `03-Architectures/`, `04-Experiments/`, and `05-Glossary/` if applicable.

**What Claude does:**
- Extracts metadata (title, authors, year, arxiv link)
- Fills the full paper template (summary, contributions, method, benchmarks, limitations)
- Adds `[[wikilinks]]` to existing concepts
- Creates new concept stubs if the paper introduces ideas not yet in the wiki
- Tags everything appropriately

### 2. Expand a Stub Concept

When you have a partially-filled concept note:

> The note `02-Concepts/Attention.md` is a stub. Expand it with: intuition, math, common variants (MHA/GQA/MLA), and link to relevant papers in `01-Papers/`.

### 3. Cross-Linking Pass

After adding several papers:

> Scan `01-Papers/` for any plain mentions of concepts that exist in `02-Concepts/` but aren't yet wikilinked. Update the files to use `[[wikilinks]]`.

### 4. Generate / Refresh a MOC

> Update `00-MOC/Papers-MOC.md` to include all papers, grouped by topic tag, sorted by date_added descending.

### 5. Process the Inbox

> Look at everything in `99-Inbox/`. For each item, decide which folder it belongs in (Papers, Concepts, Architectures, Experiments), create the proper note using the right template, then update or delete the inbox entry.

### 6. Compare Multiple Papers

> Use `agents/critical-reviewer.md`. Compare the methods in `01-Papers/Efficient Long CoT Reasoning in Small Language Models.md` and `01-Papers/<other paper>.md`. Create a critical review in `06-Critical-Reviews/` analyzing differences in: thesis, method, assumptions, training data, evaluation, results, tradeoffs, and engineering takeaways.

### 7. Plan an Experiment

> Based on `01-Papers/<paper>.md`, draft an experiment plan in `04-Experiments/` to reproduce the main result on Llama-3.1-8B. Include: hypothesis, setup, method, expected results table, risks.

### 8. Build a Topic Cluster

When starting a new research area:

> I want to study LLM memory systems. Create stub notes in `02-Concepts/` for: KV Cache, Long Context Methods, RAG, Episodic Memory, and Memory Compression. Each should follow the template with at least one paragraph in the Definition and Intuition sections.

### 9. Daily Research Log

> Create a daily note for today in `04-Experiments/daily/` summarizing what I read and any open questions. Link the new note from `00-MOC/Home.md`.

### 10. Audit & Quality Check

> List all notes in `01-Papers/` where the `status` frontmatter is still `draft`. For each, suggest what's missing.

### 11. Critical Review

After one or more source notes have passed review:

> Use `agents/critical-reviewer.md`. Review `01-Papers/<paper>.md` against related notes in `02-Concepts/`, `03-Architectures/`, and `04-Experiments/`. Create a critical review in `06-Critical-Reviews/` using `06-Critical-Reviews/_template.md`.

---

## 🤝 Worker / Reviewer Flow

A two-role pipeline for processing papers with a built-in quality gate, plus a separate critical-review synthesis role. Full role definitions and copy-paste prompts live in:

- `agents/worker.md`
- `agents/reviewer.md`
- `agents/critical-reviewer.md`

- **Worker** — reads and analyses the paper, fills the note (the existing "Process a Paper" flow).
- **Reviewer** — independently verifies the Worker's output against the source paper before it's trusted.
- **Critical Reviewer** — runs after source notes are reviewed; compares methods/models and records opinionated insight in `06-Critical-Reviews/`.

The `status` frontmatter field is the handoff baton:
`draft` → **`needs-review`** (Worker done) → `reviewed` (Reviewer passed) *or* `in-progress` (Reviewer kicked it back with fixes).

> ⚠️ **Always run the Reviewer in a NEW chat.** A reviewer sharing the worker's context just rubber-stamps it. A fresh chat forces it to re-open the paper and re-derive every number — which is how hallucinated benchmarks get caught.

Critical reviews do **not** replace this quality gate. Keep paper notes faithful and evidence-checked first; then use `06-Critical-Reviews/` for synthesis, comparisons, and judgment.

### 🅰️ Use the Worker only
When you just want the note written fast and you'll eyeball it yourself.

> Use `agents/worker.md`. Process the attached paper into `01-Papers/`. Set `status: needs-review` when done.

Result: a fully-filled note marked `needs-review`. Nothing is auto-trusted — you can promote it to `reviewed` manually whenever you're happy.

### 🅱️ Use the Reviewer only
When a note already exists (Worker-made, or older notes you want audited) and you only want it checked.

> Use `agents/reviewer.md`. Review `01-Papers/<title>.md` against `<arxiv link / attached PDF>`. Give a verdict + checklist, then update `status` accordingly.

Run this in a fresh chat. Good for batch-auditing: *"List all `01-Papers/` notes with `status: needs-review`, then review each."*

### 🆎 Use both (full pipeline)
The normal flow for a new paper.

1. **Chat 1 — Worker:** use `agents/worker.md` to process the attached paper into `01-Papers/` → ends at `status: needs-review`.
2. **Chat 2 (new) — Reviewer:** use `agents/reviewer.md` to review `01-Papers/<title>.md` against the paper → PASS sets `reviewed`; FAIL sets `in-progress` + a fix list under `## Review`.
3. **If FAIL:** hand the fix list back to a Worker chat: *"Apply the fixes under `## Review` in `01-Papers/<title>.md`, then set `status: needs-review`."* Re-review until it passes.

**Hands-off variant:** add *"if FAIL, apply the fixes yourself then re-check"* to the Reviewer prompt — one chat does the loop, but you lose the human checkpoint.

### 🧠 Use the Critical Reviewer
When you want a higher-level view across reviewed notes:

> Use `agents/critical-reviewer.md`. Compare `01-Papers/<paper A>.md`, `01-Papers/<paper B>.md`, and any relevant architecture/concept notes. Create `06-Critical-Reviews/<review title>.md`.

Result: an opinionated review note that cites the source notes, compares tradeoffs, calls out failure modes, and records your engineering/research takeaways.

## ✍️ Prompting Principles (for best results)

### ✅ Be explicit about paths
**Bad:** "Add this to my notes"
**Good:** "Create `01-Papers/<title>.md` using `01-Papers/_template.md`"

### ✅ Reference existing notes
**Bad:** "Talk about attention"
**Good:** "Expand `02-Concepts/Attention.md`, link to relevant papers in `01-Papers/`"

### ✅ Specify tone & depth
**Bad:** "Summarize this paper"
**Good:** "Summarize in technical depth for a Lead Data Engineer with ML background; include math where it clarifies"

### ✅ Ask for structured output
**Bad:** "Compare these methods"
**Good:** "Compare in a markdown table with columns: Method, Complexity, Strength, Weakness"

### ✅ Chain multi-step requests
**Bad:** Many small prompts
**Good:** "1) Process this paper into `01-Papers/`, 2) extract any new concepts into `02-Concepts/`, 3) update `00-MOC/Papers-MOC.md`"

### ✅ Use the templates
Templates in each folder enforce consistent frontmatter (tags, status, dates) which makes Dataview queries in the MOCs actually work. Always reference them.

---

## 🏷️ Tag Conventions

Keep tags consistent so Dataview queries work cleanly.

| Tag | Use in |
|-----|--------|
| `paper` | All notes in `01-Papers/` |
| `concept` | All notes in `02-Concepts/` |
| `architecture` | All notes in `03-Architectures/` |
| `experiment` | All notes in `04-Experiments/` |
| `glossary` | All notes in `05-Glossary/` |
| `critical-review` | All notes in `06-Critical-Reviews/` |
| `SLM` | Small Language Model topics |
| `agentic` | Agents, tool use, planning |
| `memory` | LLM memory, KV cache, context |
| `efficiency` | Inference / training efficiency |
| `reasoning` | CoT, planning, math reasoning |
| `alignment` | RLHF, DPO, safety |

### Status values
- `draft` — initial dump, needs work
- `needs-review` — Worker finished; awaiting Reviewer (see `agents/reviewer.md`)
- `in-progress` — being actively expanded / Reviewer sent it back with fixes
- `reviewed` — read, structured, and verified against the source
- `stable` — won't change much

## 🔍 Research Focus Areas (current)

- **Small Language Models (SLMs)** — 7B-class models, distillation, on-device
- **Agentic Systems** — tool use, planning, multi-step reasoning
- **LLM Memory** — KV cache optimization, long context, retrieval
- **Efficient Reasoning** — CoT pruning, test-time compute trade-offs
- **On-device Inference** — quantization, hardware-aware deployment

---

## 🗓️ Weekly Workflow

1. **Daily** — drop interesting links in `99-Inbox/`
2. **Weekly** — process inbox, expand stubs, run an experiment
3. **Monthly** — review MOCs, write a critical review in `06-Critical-Reviews/`, push to GitHub

---

## 🚀 Quick Start Commands for Claude

Copy these into Claude Desktop when you need them:

```
"Process the attached paper into my LLM wiki"
"Expand the stub at 02-Concepts/<name>.md"
"Cross-link all papers in 01-Papers/ to concepts"
"Update all MOCs in 00-MOC/"
"Process my inbox"
"Draft an experiment plan for 04-Experiments/ based on <paper>"
"Create a critical review in 06-Critical-Reviews/ comparing <paper A> and <paper B>"
"Audit 01-Papers/ — list notes with status=draft"
```

**Worker / Reviewer flow** (see `agents/worker.md` and `agents/reviewer.md`):

```
# Worker only
"Use agents/worker.md. Process the attached paper into 01-Papers/."

# Reviewer only (run in a NEW chat)
"Use agents/reviewer.md. Review 01-Papers/<title>.md against <paper link>."

# Find what needs reviewing
"List all 01-Papers/ notes with status: needs-review."

# Apply review fixes
"Apply the fixes under ## Review in 01-Papers/<title>.md, then set status: needs-review."
```

## 📦 Stack

- **Obsidian** — note editor + graph view
- **Local REST API** plugin — exposes vault as HTTPS API
- **Claude Desktop** — connects to vault via MCP
- **Templater** plugin — note templates
- **Dataview** plugin — query notes like a database
- **Smart Connections** — semantic search across notes
- **Obsidian Git** — auto-sync to GitHub

---

*Maintained by Nathan. Last updated: 2025-06-13.*
