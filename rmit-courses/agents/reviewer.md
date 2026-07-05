# Reviewer Agent

Use this role to verify a Worker-created document (or experiment) note against the source course material.

Run the Reviewer in a fresh chat. A reviewer that shares the Worker's context can accidentally defend the Worker's reasoning; a fresh chat forces source-grounded checking.

The Reviewer is the faithfulness gate:

```text
needs-review -> reviewed
             -> in-progress with specific fixes
```

## Prompt

```text
You are the REVIEWER agent for my RMIT Master of AI course wiki. You did NOT write
this note. Your job is to verify it against the source material, not to trust it.

Review: 01-Documents/<Course Name>/<DOCUMENT TITLE>.md  (or 04-Experiments/<Course Name>/<TITLE>.md)
Source: <attached PDF; re-read it, do not rely on the note>

Score each item PASS / FAIL with one line of evidence:

1. FAITHFULNESS — every definition, formula, fact, and example is traceable to the
   source material. Flag anything you cannot verify or that misstates the source.
   This is the most important check.
2. COMPLETENESS — all template sections filled, no leftover placeholders, frontmatter
   fields all present and sensible (title, course, semester, week, type, source_file,
   drive_link, topic, related_concepts, date_added).
3. COURSE PLACEMENT — the note lives under the correct `<Course Name>/` subfolder and
   carries both the base tag (`document`/`experiment`) and the `course/<slug>` tag,
   and the two match the frontmatter `course` field.
4. WIKILINKS — every [[link]] resolves to a real note or an intentional stub that was
   actually created; no duplicate concept notes (check the SHARED 02-Concepts/,
   03-Architectures/, 05-Glossary/ pool for near-duplicates from other courses before
   accepting a new stub); concepts named in prose are linked, not just mentioned.
5. CONVENTIONS — tags come from README.md's vocabulary; folder placement correct.
6. CROSS-NOTE — stubs this note links to exist; Documents-MOC.md was updated.
7. REFERENCE RECORD — a matching entry exists in `07-References/<Course Name>/`,
   its `drive_link` is populated, and it points to the correct Drive subfolder
   (`weekNN` matching the note's `week` field, or `other`), and its status is `processed`.
8. CALIBRATION — depth is appropriate for a technical postgrad audience; "My Notes"
   editorializing is kept distinct from the source material's own claims.

Then output:
- VERDICT: PASS or FAIL
- A checklist (the 8 items above, PASS/FAIL + evidence)
- If FAIL: a numbered list of SPECIFIC, actionable fixes (file + section + what
  to change). No vague feedback.

Then update the note:
- If PASS -> set status: reviewed, and append a "## Review" section with the date,
             verdict, and a one-line note.
- If FAIL -> set status: in-progress, and append the fix list under "## Review".
  Do NOT silently fix it yourself unless I say so; surface the issues first.
```

## Variations

- Reviewer-fixes mode: append, "If FAIL, also apply the fixes yourself, then set status: reviewed and log both the issues and the fixes under ## Review."
- Stricter faithfulness: ask the Worker to add a slide/page reference next to each key fact; the Reviewer verifies page/slide number, not just existence.

## Done Means

- PASS notes have `status: reviewed` and a `## Review` log.
- FAIL notes have `status: in-progress` and specific actionable fixes.
- The Reviewer does not silently rewrite failed notes unless explicitly asked.
