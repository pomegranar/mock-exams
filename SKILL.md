---
name: mock-exams
description: Generate a mock midterm or final exam (LaTeX → PDF) from a course's lecture materials. Produces a multi-section exam with answer key and formula sheet, compiled via latexmk.
---

# Mock Exam Generator

Build a printable mock exam in PDF form from a course's lecture materials (slides, lecture notes, transcripts, textbooks). The exam should test recognition, understanding, and application — not just recall.

## When to use

User asks to "create a mock exam," "make a practice midterm," "generate a final exam," or similar, and points at a folder of lecture content.

## Inputs to gather

Before generating, confirm:
1. **Syllabus** (if present) — check the source folder for a `syllabus` file first. It gives essential context: course level, difficulty expectations, prerequisites, whether the course is math-heavy or conceptual, and anything the instructor flags as "key topics" or "learning objectives." This shapes question tone and difficulty before you even open the lectures.
2. **Source folder** — where the lecture content lives (PDFs, ..txt, .md, slides).
2. **Scope** — midterm (subset of lectures) or final (all lectures). Confirm which lectures are in scope.
3. **Output folder** — where to drop the `.tex` and `.pdf`. Default: `./Mock_exams/`.
4. **Course code & term** — for the header (e.g., "STATS 202 — Spring 2026").
5. **Total points & length** — defaults: 100 pts / ~15 pages for midterm, 150 pts / ~20 pages for final.

## Process

1. **Check for a syllabus.** If a syllabus file exists in the source folder, read it first. Note the course level, stated learning objectives, key topics, and any instructor emphasis. Use this to calibrate question difficulty and framing before generating.
2. **Survey the lectures.** Use `find` + `read` to list every lecture file. Read each one to extract: key definitions, frameworks, formulas, computational examples, conceptual themes. Cover *every* lecture — do not rely on summaries that skip files. If the user later asks "why didn't you cover X?", that's a process failure.
2. **Outline the exam.** Pick a question count per section that hits the target points and gives every lecture at least one question. See "Default structure" below.
3. **Write the `.tex` file** using the `exam` class. Follow the template in `template.tex`.
4. **Compile** with `latexmk -pdf <file>.tex` from the output folder. Check page count and watch for overflow/header-overlap warnings.
5. **Clean up** auxiliaries with `latexmk -c`.
6. **Report** to the user: page count, point breakdown, file path.

## Default structure

**Midterm (100 pts, ~15 pages):**
- Part I: Multiple Choice — 12 questions × 2 pts = 24 pts
- Part II: True/False (with one-line justification) — 6 × 3 pts = 18 pts
- Part III: Short Answer — 4 × 7 pts = 28 pts
- Part IV: Problems (multi-part, computational) — 3 × 10 pts = 30 pts

**Final (150 pts, ~20 pages):**
- Part I: Multiple Choice — 16 × 2 pts = 32 pts
- Part II: True/False — 8 × 3 pts = 24 pts
- Part III: Short Answer — 6, varying = 44 pts
- Part IV: Problems — 5, varying = 50 pts

Mix question kinds to test different skills:
- **Recognition** → MC with plausible distractors drawn from related concepts in the lectures.
- **Understanding** → T/F with brief justification; conceptual short-answer.
- **Application** → numerical problems (probability computation, MLE/ERM derivation, time-series forecast, Bayes-net inference, etc.).
- **Synthesis** → cross-lecture problems (e.g., "explain X using framework Y from week Z").

Include a **formula sheet** before the questions and a **full answer key with worked solutions** on the last pages.

## LaTeX template

See `template.tex` for the boilerplate. Key features:
- `\documentclass[12pt,answers]{exam}` — toggle `answers` ↔ `noanswers` (or use `\printanswers` / `\noprintanswers`).
- `\firstpageheader{}{}{}` — **leave empty.** Adding text here causes overlap with the manual title block.
- Manual scoring table (do not use `\gradetable` — it errors on `[parts]` indexing and overflows).
- TikZ for diagrams (Bayes nets, neural-net schematics): `\usetikzlibrary{arrows.meta, positioning, calc}` — `calc` is required for `$(A)!0.5!(B)$` coordinate math.

## Common pitfalls (learned the hard way)

| Symptom | Cause | Fix |
|---|---|---|
| `Something's wrong--perhaps a missing \item` | Empty `\begin{parts}\end{parts}` block | Delete the empty block, or add at least one `\part` |
| `Grade and point tables can be indexed...` error | `\gradetable[h][parts]` | Use `\gradetable[h][questions]`, or replace with a manual `tabular` |
| Header text overlaps the title | `\firstpageheader{...}{...}{...}` rendering on top of a manual `\begin{center}...\end{center}` title | Set `\firstpageheader{}{}{}` |
| `You need to say \usetikzlibrary{calc}` | Using `$(A)!0.5!(B)$` coordinate syntax | Add `calc` to `\usetikzlibrary{...}` |
| Scoring table overflows page | Too many columns, or `\gradetable` auto-expansion | Use the 3-column manual table from `template.tex` |
| Missed a lecture | Trusted an agent summary that skipped a file | Always enumerate the lecture folder yourself; read each file |

## Verification checklist

Before declaring done:
- [ ] Every lecture in scope is referenced by at least one question (cross-check against the file listing).
- [ ] Question types are mixed (recognition + understanding + application).
- [ ] Answer key includes *worked solutions* for problems, not just final answers.
- [ ] Formula sheet present.
- [ ] PDF compiles cleanly (no overfull `\hbox` warnings on the title page; no missing references).
- [ ] First page renders without header/title overlap — open the PDF and look.
- [ ] Page count is in the expected range.

## Files in this skill

- `template.tex` — the LaTeX boilerplate to copy and adapt.
- `SKILL.md` — this file.
