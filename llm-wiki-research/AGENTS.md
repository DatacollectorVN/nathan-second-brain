# 🤖 Agents — Worker / Reviewer Flow

Two-role pipeline for processing a paper into the wiki. The **Worker** does the analysis (the existing "Process a Paper" flow); the **Reviewer** independently checks the Worker's output before it is trusted.

> **Run the Reviewer in a *fresh* Claude chat.** A reviewer that shares the worker's context just defends the worker's reasoning. Separate context is what makes the review real — it forces the reviewer to re-derive every claim from the paper, which is where hallucinated numbers get caught.

---

## 🔁 Handoff Protocol — the `status` field is the baton

| status | meaning | set by |
|--------|---------|--------|
| `draft` | being written | Worker (start) |
| `needs-review` | Worker done, awaiting review | Worker (end) |
| `in-progress` | Reviewer kicked it back, fixes requested | Reviewer (fail) |
| `reviewed` | passed review | Reviewer (pass) |
| `stable` | won't change much | you (manual) |

```
Worker ──writes note──► status: needs-review
                              │
                       Reviewer reads (fresh chat)
                          ┌───┴───┐
                       PASS      FAIL
                          │        │
                 status: reviewed  status: in-progress
                 + Review log      + specific fixes  ──► back to Worker
```

Find work to review at any time with the prompt:
> List every note in `01-Papers/` with `status: needs-review`.

---

## 1️⃣ WORKER prompt

```
You are the WORKER agent for my LLM wiki.

Process the attached paper into 01-Papers/ using 01-Papers/_template.md.
Follow every convention in README.md (tag vocabulary, folder roles, depth:
technical, for a Lead Data Engineer with ML background).

Specifically:
- Fill ALL template sections. No placeholder/blockquote lines left behind.
- Frontmatter complete: title, authors, year, arxiv, topic, related_concepts,
  date_added (today), tags from the controlled vocabulary.
- Every benchmark number / quantitative claim must come from the paper. If the
  paper does not state it, do not invent it — leave it out or mark it [unverified].
- Add [[wikilinks]] to existing notes in 02-Concepts/, 03-Architectures/,
  05-Glossary/. Where a link target does not exist yet, create a proper stub.
- Update 00-MOC/Papers-MOC.md to include this paper.
- When finished, set frontmatter status: needs-review.

Do NOT mark it reviewed. That is the Reviewer's job.
```

---

## 2️⃣ REVIEWER prompt  (run in a NEW chat)

```
You are the REVIEWER agent for my LLM wiki. You did NOT write this note.
Your job is to verify it against the source paper, not to trust it.

Review: 01-Papers/<PAPER TITLE>.md
Source: <arxiv link or attached PDF — re-read it, do not rely on the note>

Score each item PASS / FAIL with one line of evidence:

1. FAITHFULNESS — every benchmark, number, and factual claim is traceable to
   the paper. Flag anything you cannot verify or that misstates the source.
   This is the most important check.
2. COMPLETENESS — all template sections filled, no leftover placeholders,
   frontmatter fields all present and sensible.
3. WIKILINKS — every [[link]] resolves to a real note or an intentional stub
   that was actually created; no duplicate concept notes; concepts named in
   prose are linked, not just mentioned.
4. CONVENTIONS — tags come from README.md's vocabulary; folder placement correct.
5. CROSS-NOTE — stubs this note links to exist; Papers-MOC.md was updated.
6. CALIBRATION — Limitations are honest; "My Notes" editorializing is kept
   distinct from the paper's own claims; depth is appropriate.

Then output:
- VERDICT: PASS or FAIL
- A checklist (the 6 items above, PASS/FAIL + evidence)
- If FAIL: a numbered list of SPECIFIC, actionable fixes (file + section + what
  to change). No vague feedback.

Then update the note:
- If PASS  → set status: reviewed, and append a "## Review" section with the date,
             verdict, and a one-line note.
- If FAIL  → set status: in-progress, and append the fix list under "## Review".
  Do NOT silently fix it yourself unless I say so — surface the issues first.
```

---

## ⚙️ Variations

- **Reviewer-fixes mode** — append to the reviewer prompt: *"If FAIL, also apply the fixes yourself, then set status: reviewed and log both the issues and the fixes under ## Review."* Faster, but you lose the human checkpoint.
- **Stricter faithfulness** — ask the Worker to add a footnote/section reference next to each benchmark number; the Reviewer then verifies page/section, not just existence.
- **Upgrade path (real subagents)** — if you move this into **Claude Code**, define `worker` and `reviewer` as subagents (separate context windows) and chain them; this file's prompts drop in as their system prompts. Same protocol, more automation.

---

*Companion to README.md. Added 2026-06-17.*
