# 🎓 RMIT Master of AI — Course Wiki

A personal knowledge base for RMIT's Master of Artificial Intelligence coursework —
built with Obsidian + Claude (via MCP). Same three-agent pipeline as
`second-brain/llm-wiki-research`, retargeted at course material instead of research
papers. **Hybrid structure:** raw, high-volume, course-specific material
(`01-Documents/`, `04-Experiments/`, `07-References/`) is split into per-course
subfolders; the knowledge-graph layer (`02-Concepts/`, `03-Architectures/`,
`05-Glossary/`, `06-Critical-Reviews/`) stays SHARED across every course, so concepts
that show up in more than one subject (e.g. gradient descent in both Deep Learning
and Reinforcement Learning) live in exactly one note that every relevant course links into.

---

## 📁 Folder Structure & Roles

```
rmit-courses/
├── 00-MOC/                  → Maps of Content (index dashboards)
├── 01-Documents/            → Course material, split per course
│   └── <Course Name>/       →   e.g. 01-Documents/AI/
├── 02-Concepts/             → SHARED atomic concept notes — the building blocks
├── 03-Architectures/        → SHARED model families, algorithms & systems taught
├── 04-Experiments/          → Labs & assignments, split per course
│   └── <Course Name>/
├── 05-Glossary/             → SHARED quick definitions (2-3 sentences)
├── 06-Critical-Reviews/     → SHARED comparative reviews, cross-course synthesis
├── 07-References/           → Raw attachment log, split per course
│   └── <Course Name>/       →   one reference record per PDF received
├── agents/                  → Worker / Reviewer / Critical Reviewer prompts (course-agnostic)
└── 99-Inbox/                → Raw capture, processed weekly
```

### 00-MOC — Maps of Content
Dashboard notes that index everything via Dataview queries. Start every session by opening `Home.md`, which also holds the "Courses This Semester" table — the source of truth the Worker checks when a course name isn't given explicitly.

### 01-Documents — Course material (per course)
One note per lecture deck, reading, assigned paper, tutorial, lab sheet, or past exam, filed under `01-Documents/<Course Name>/`. Each follows `01-Documents/_template.md`: summary, key points, core content, examples, limitations, my notes, and a **Source** section recording the original filename and its Google Drive link. Tagged `document` + `course/<slug>`. **This is the biggest folder.**

### 02-Concepts — Atomic ideas (shared across all courses)
Reusable building blocks that course material references: Attention, Search Algorithms, Bayesian Inference, RLHF, etc. One concept = one note, regardless of which course introduced it. Documents link *into* this folder via `[[wikilinks]]`. **Before creating a new concept note, check here first** — the same idea often gets taught from a different angle in a different course, and that's a link, not a duplicate note. Over time this becomes a personal course encyclopedia — useful for exam revision across subjects.

### 03-Architectures — Specific models & systems (shared)
One note per model family, algorithm, or system: Transformer, CNN families, specific RL algorithms, planning systems. Different from concepts — this is about *specific implementations*, design choices, and training/operating details.

### 04-Experiments — Labs & hands-on work (per course)
Assignments, lab exercises, reproductions, and your own experiments, filed under `04-Experiments/<Course Name>/`. Tracks hypothesis → setup → results → conclusion. Tagged `experiment` + `course/<slug>`.

### 05-Glossary — Quick reference (shared)
2-3 sentence definitions only. Link here when you need a fast reminder of a term without reading a full concept note.

### 06-Critical-Reviews — Comparative judgment layer (shared)
Reviews of models, methods, and topic clusters — including **across courses**, which is the main reason this stays unified rather than split. Explicitly analytical: compare alternatives, surface tradeoffs, identify failure modes, and record exam-prep or engineering takeaways. Use after source notes are processed and, ideally, reviewed.

### 07-References — Raw attachment log (per course)
Every PDF you send in chat, or that lands in `99-Inbox/`, gets a lightweight reference record here at `07-References/<Course Name>/<original filename>.md` — logged the moment it's received, before (or independent of) full processing into `01-Documents/`. Tracks: source filename, course, week (or `other`), date received, Google Drive link (once archived), and a link to the resulting note. Status moves `received` → `archived` → `processed`. This is your raw-material catalog — a quick way to see "what have I actually received for Course X" without opening every processed note.

### agents — Agent role instructions (course-agnostic)
Reusable prompts for the three-agent workflow:
`agents/worker.md`, `agents/reviewer.md`, and `agents/critical-reviewer.md`.
The same three agents work across every course — the course itself is resolved per
run, not baked into the prompt.

### 99-Inbox — Raw capture
Paste links, PDFs-to-process-later, or half-formed ideas here. Process into proper folders weekly. The `99` prefix pushes it to the bottom of the file tree.

---

## 🎓 Resolving the Course (mandatory, every run)

Because `01-Documents/`, `04-Experiments/`, and `07-References/` are split per course,
the Worker needs a course before it creates anything:

1. **You state it explicitly** ("this is for AI", "course: Deep Learning") → used as-is.
2. **You don't state it** → the Worker checks `00-MOC/Home.md` → "Courses This
   Semester" and the existing per-course subfolders for a topical match.
   - Confident single match → used, but the Worker tells you which course it assumed.
   - No match / multiple plausible matches / genuine uncertainty → the Worker stops
     and asks you, rather than guessing or defaulting to whatever course was used last.

The resolved course name becomes both the subfolder (`01-Documents/<Course Name>/`)
and the tag slug (`course/<slug>`, lowercased and hyphenated).

---

## ☁️ Google Drive Archive

The vault holds the structured notes; Google Drive holds the raw source PDFs, organized
per course to mirror the semester/week structure. This happens automatically as the
last step of the Worker pipeline — see `agents/worker.md`.

**Known course → Drive root mapping** (add a row whenever a new course starts):

| Course | Drive root folder |
|--------|--------------------|
| AI | https://drive.google.com/drive/folders/1MGy-jXr34En2BUZxzliG5AQxX37ilQMB |

**Rule**, once the course's Drive root is known:
- Explicit week number given → the PDF is uploaded to the `weekNN` subfolder under
  that course's Drive root, zero-padded to two digits (`week01`, `week04`, `week12`,
  ...). The subfolder is created first if it doesn't exist.
- No week number given → the PDF is uploaded to the `other` subfolder under that
  course's Drive root (created if missing).
- The original filename is preserved on Drive.
- The resulting Drive link is written back into the note's `drive_link` frontmatter
  field, its **Source** section, and the `07-References/` record.

**If the course's Drive root isn't in the table yet**, the Worker asks you for it
instead of guessing or reusing another course's folder — then adds the row here for
next time.

You don't need to ask for the upload separately — just attach the PDF and (optionally)
mention the course and week when you invoke the Worker.

---

## 🔗 How They Connect

```
99-Inbox (raw)
    ↓ process weekly
07-References/<Course> ── logs receipt of ──► 01-Documents/<Course>
                                                    │
                                          ┌─────────┴─────────┐
                                          ▼                   ▼
                                   02-Concepts (shared) 03-Architectures (shared)
                                          ▲                   ▲
04-Experiments/<Course> ─── tests ───────┘                   │
                                                               │
05-Glossary (shared) ◄── defines terms ─────────────── everywhere

Reviewed source notes (any course) ─────► 06-Critical-Reviews (shared)
                              compares / critiques / synthesizes across courses

01-Documents/<Course> ── archived as PDF ──► Google Drive (<Course>/weekNN or other)
```

The power is in `[[wikilinks]]`. Obsidian builds a knowledge graph automatically — open **Graph View** (Cmd+G) to see your course landscape visually, including where different courses converge on the same concept.

---

## 🤖 Working with Claude (via MCP)

This vault is connected to Claude Desktop through the Obsidian Local REST API MCP server, alongside a Google Drive connector for the archive step. Below are battle-tested prompt patterns.

### 1. Process a Document (most common)

**Attach the PDF.** Then say, with course and week when you have them:

> Process this for AI, week 4.

or, if the course is already obvious from context and there's no specific week:

> Process this into my RMIT course wiki — no week, file under other.

If you don't say the course and the Worker can't confidently infer it, it will ask you before doing anything.

**What Claude does:**
- Resolves the course (see "Resolving the Course" above)
- Logs a reference record in `07-References/<Course>/`
- Extracts metadata (title, type) and fills the full document template
- Adds `[[wikilinks]]` to existing concepts/architectures/glossary entries — checking the shared pool first — creating stubs where needed
- Tags the note `document` + `course/<slug>`, sets `status: needs-review`
- Uploads the PDF to the correct Google Drive subfolder and records the link in both the note and the reference record

### 2. Expand a Stub Concept

> The note `02-Concepts/Bayesian Inference.md` is a stub. Expand it with: intuition, math, common variants, and link to relevant documents in `01-Documents/`.

### 3. Cross-Linking Pass

> Scan `01-Documents/` for any plain mentions of concepts that exist in `02-Concepts/` but aren't yet wikilinked. Update the files to use `[[wikilinks]]`.

### 4. Generate / Refresh a MOC

> Update `00-MOC/Documents-MOC.md` to include all documents, grouped by course and week, sorted by week ascending.

### 5. Process the Inbox

> Look at everything in `99-Inbox/`. For each item, resolve the course, decide which folder it belongs in, create the proper note using the right template, then update or delete the inbox entry.

### 6. Compare Multiple Documents (including across courses)

> Use `agents/critical-reviewer.md`. Compare `01-Documents/AI/<lecture A>.md` and `01-Documents/<Other Course>/<lecture B>.md`. Create a critical review in `06-Critical-Reviews/` analyzing differences in: thesis, method, assumptions, data, evaluation, tradeoffs, and exam-relevant takeaways.

### 7. Plan a Lab / Assignment

> Based on `01-Documents/AI/<lecture>.md`, draft a lab plan in `04-Experiments/AI/` to implement the method taught. Include: hypothesis, setup, method, expected results table, risks.

### 8. Build a Topic Cluster

> I'm starting the Reinforcement Learning module in AI. Create stub notes in `02-Concepts/` for: MDPs, Value Iteration, Policy Gradients, Q-Learning. Each should follow the template with at least one paragraph in Definition and Intuition.

### 9. Weekly Study Log

> Create a note for this week in `04-Experiments/AI/weekly-logs/` summarizing what I covered and any open questions. Link it from `00-MOC/Home.md`.

### 10. Audit & Quality Check

> List all notes in `01-Documents/` where `status` is still `draft` or `needs-review`. For each, suggest what's missing.

### 11. What have I actually received for a course?

> Show me everything in `07-References/AI/` — which ones are still `received` (not archived to Drive) or not yet `processed` into a document note.

---

## 🤝 Worker / Reviewer Flow

A two-role pipeline for processing course material with a built-in quality gate, plus a separate critical-review synthesis role. Full role definitions and copy-paste prompts live in:

- `agents/worker.md`
- `agents/reviewer.md`
- `agents/critical-reviewer.md`

- **Worker** — resolves the course, logs the reference, reads the PDF, fills the note, and archives the PDF to Google Drive.
- **Reviewer** — independently verifies the Worker's output against the source material before it's trusted, including course placement/tagging and the reference record.
- **Critical Reviewer** — runs after source notes are reviewed; compares topics/methods across weeks *and courses*, recording opinionated, exam-relevant insight in `06-Critical-Reviews/`.

The `status` frontmatter field is the handoff baton:
`draft` → **`needs-review`** (Worker done) → `reviewed` (Reviewer passed) *or* `in-progress` (Reviewer kicked it back with fixes).

> ⚠️ **Always run the Reviewer in a NEW chat.** A reviewer sharing the worker's context just rubber-stamps it. A fresh chat forces it to re-open the material and re-derive every fact — which is how mistakes get caught.

Critical reviews do **not** replace this quality gate. Keep document notes faithful and evidence-checked first; then use `06-Critical-Reviews/` for synthesis and judgment.

### 🅰️ Use the Worker only
When you just want the note written (and PDF archived) fast, and you'll eyeball it yourself.

> Use `agents/worker.md`. Process the attached PDF into `01-Documents/AI/` for week 4. Set `status: needs-review` when done.

### 🅱️ Use the Reviewer only
When a note already exists and you only want it checked.

> Use `agents/reviewer.md`. Review `01-Documents/AI/<title>.md` against the attached PDF. Give a verdict + checklist, then update `status` accordingly.

Run this in a fresh chat. Good for batch-auditing: *"List all `01-Documents/` notes with `status: needs-review`, then review each."*

### 🆎 Use both (full pipeline)
The normal flow for a new piece of course material.

1. **Chat 1 — Worker:** use `agents/worker.md` to process the attached PDF (with course and, optionally, week number) → ends at `status: needs-review`, PDF archived to Drive, reference record logged.
2. **Chat 2 (new) — Reviewer:** use `agents/reviewer.md` to review `01-Documents/<Course>/<title>.md` against the PDF → PASS sets `reviewed`; FAIL sets `in-progress` + a fix list under `## Review`.
3. **If FAIL:** hand the fix list back to a Worker chat: *"Apply the fixes under `## Review` in `01-Documents/<Course>/<title>.md`, then set `status: needs-review`."* Re-review until it passes.

**Hands-off variant:** add *"if FAIL, apply the fixes yourself then re-check"* to the Reviewer prompt — one chat does the loop, but you lose the human checkpoint.

### 🧠 Use the Critical Reviewer
When you want a higher-level view across reviewed notes — great for exam revision, including tying two different courses together:

> Use `agents/critical-reviewer.md`. Compare `01-Documents/AI/<lecture A>.md`, `01-Documents/<Other Course>/<lecture B>.md`, and any relevant concept/architecture notes. Create `06-Critical-Reviews/<review title>.md`.

---

## ✍️ Prompting Principles (for best results)

### ✅ State the course, or say when it's ambiguous
**Bad:** "Process this PDF"
**Good:** "Process this PDF for AI, week 6" — or, if you're not sure the Worker can tell: "Process this PDF — not sure which course, check and ask me if unclear"

### ✅ Be explicit about paths
**Bad:** "Add this to my notes"
**Good:** "Create `01-Documents/AI/<title>.md` using `01-Documents/_template.md`"

### ✅ Reference existing notes
**Bad:** "Talk about attention"
**Good:** "Expand `02-Concepts/Attention.md`, link to relevant documents in `01-Documents/`"

### ✅ Specify tone & depth
**Bad:** "Summarize this lecture"
**Good:** "Summarize in technical depth for a Lead Data Engineer with ML background; include math where it clarifies"

### ✅ Ask for structured output
**Bad:** "Compare these methods"
**Good:** "Compare in a markdown table with columns: Method, Complexity, Strength, Weakness"

### ✅ Chain multi-step requests
**Bad:** Many small prompts
**Good:** "1) Process this for AI week 4, 2) extract any new concepts into `02-Concepts/`, 3) update `00-MOC/Documents-MOC.md`"

### ✅ Use the templates
Templates in each folder enforce consistent frontmatter (tags, status, dates) which makes Dataview queries in the MOCs actually work. Always reference them.

---

## 🏷️ Tag Conventions

Keep tags consistent so Dataview queries work cleanly.

| Tag | Use in |
|-----|--------|
| `document` | All notes in `01-Documents/` |
| `concept` | All notes in `02-Concepts/` |
| `architecture` | All notes in `03-Architectures/` |
| `experiment` | All notes in `04-Experiments/` |
| `glossary` | All notes in `05-Glossary/` |
| `reference` | All notes in `07-References/` |
| `critical-review` | All notes in `06-Critical-Reviews/` |
| `course/<slug>` | Every note in `01-Documents/`, `04-Experiments/`, and `07-References/` — e.g. `course/ai`. Lowercase, hyphenated version of the course name. |

### Document `type` values (frontmatter)
- `lecture-slides` — weekly lecture deck
- `reading` — assigned reading / textbook excerpt
- `textbook-chapter` — full chapter
- `assigned-paper` — a research paper set as course reading
- `tutorial` — tutorial/workshop sheet
- `lab-sheet` — practical lab exercise
- `past-exam` — past exam or sample questions

### Status values
- `draft` — initial dump, needs work
- `needs-review` — Worker finished; awaiting Reviewer (see `agents/reviewer.md`)
- `in-progress` — being actively expanded / Reviewer sent it back with fixes
- `reviewed` — read, structured, and verified against the source
- `stable` — won't change much

### Reference record `status` values (07-References/)
- `received` — logged, not yet archived to Drive or processed into a note
- `archived` — uploaded to Google Drive, `drive_link` populated
- `processed` — the corresponding `01-Documents/` (or `04-Experiments/`) note exists

---

## 🗓️ Weekly Workflow

1. **As material drops** — attach the PDF to a Worker chat with the course and week number; it gets logged, processed into `01-Documents/<Course>/`, and archived to Drive in one go.
2. **Weekly** — process `99-Inbox/`, expand stubs, do the week's lab in `04-Experiments/<Course>/`.
3. **Before exams / at semester milestones** — run the Critical Reviewer to synthesize across weeks (and courses) in `06-Critical-Reviews/`, review `00-MOC/` dashboards.

---

## 🚀 Quick Start Commands for Claude

Copy these into Claude Desktop when you need them:

```
"Process this PDF for AI, week 4"
"Process this PDF into my RMIT course wiki — no week, file under other"
"Expand the stub at 02-Concepts/<name>.md"
"Cross-link all documents in 01-Documents/ to concepts"
"Update all MOCs in 00-MOC/"
"Process my inbox"
"Draft a lab plan for 04-Experiments/AI/ based on <lecture>"
"Create a critical review in 06-Critical-Reviews/ comparing <doc A> and <doc B>"
"Audit 01-Documents/ — list notes with status=draft or needs-review"
"Show me everything in 07-References/AI/ that isn't processed yet"
```

**Worker / Reviewer flow** (see `agents/worker.md` and `agents/reviewer.md`):

```
# Worker only
"Use agents/worker.md. Process the attached PDF for AI, week 4."

# Reviewer only (run in a NEW chat)
"Use agents/reviewer.md. Review 01-Documents/AI/<title>.md against the attached PDF."

# Find what needs reviewing
"List all 01-Documents/ notes with status: needs-review."

# Apply review fixes
"Apply the fixes under ## Review in 01-Documents/AI/<title>.md, then set status: needs-review."
```

## 📦 Stack

- **Obsidian** — note editor + graph view
- **Local REST API** plugin — exposes vault as HTTPS API
- **Claude Desktop** — connects to vault via MCP
- **Google Drive** — PDF archive, connected via MCP
- **Templater** plugin — note templates
- **Dataview** plugin — query notes like a database
- **Obsidian Git** — auto-sync to GitHub (optional)

---

*Maintained by Nathan. RMIT Master of Artificial Intelligence.*
