# RMIT Master of AI — Course Wiki — Home

## 🗺️ Maps of Content
- [[Documents-MOC]] — All course material (lecture slides, readings, assigned papers, tutorials, labs, past exams)
- [[Concepts-MOC]] — Core concepts & theory (shared across all courses)
- [[Experiments-MOC]] — Labs, assignments & hands-on work
- [[References-MOC]] — Raw-intake catalog of every attachment received
- [[Critical-Reviews-MOC]] — Comparative reviews & exam-prep synthesis

## 🎓 Courses This Semester
> The Worker checks this list first when a course name isn't given explicitly — keep it current.

| Course Name | Folder slug (tag) | Google Drive root |
|-------------|--------------------|--------------------|
| AI | `course/ai` | [Drive folder](https://drive.google.com/drive/folders/1MGy-jXr34En2BUZxzliG5AQxX37ilQMB) |
| Monte Carlo Simulation | `course/monte-carlo-simulation` | *not yet configured — ask Nathan before archiving* |

## ☁️ Google Drive Archive
Source PDFs are archived per-course, mirroring the semester/week structure. See
`README.md` → "Google Drive Archive" for the upload rule, and `agents/worker.md` for
how it's triggered.

## 📥 Inbox
Drop raw links, PDFs-to-process-later, or half-formed notes in [[99-Inbox]]. Process weekly.

## 📊 Stats
```dataview
TABLE length(rows) AS "Count"
FROM "second-brain/rmit-courses"
WHERE file.name != "Home"
GROUP BY split(file.folder, "/")[2]
```

## 🧑‍🏫 AI-RMIT-Teacher (Claude Project)
The `AI-RMIT-Teacher` Claude Project is a persistent persona that teaches from this vault and can act as Worker / Reviewer / Critical Reviewer on request. Setup + instructions text: [[AI-RMIT-Teacher-project-instructions]].
