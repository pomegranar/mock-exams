# Mock Exam Generator

An AI-agent skill that generates printable mock midterms and finals (PDF) from a course's lecture materials. Designed for [pi agent](https://github.com/mariozechner/pi-coding-agent) but also works with **Claude** and **Codex**.

## What it does

Feed it a folder of slides, lecture notes, or transcripts and it produces:

- A structured **exam** with MC, T/F, short-answer, and problem sections
- A **formula sheet** tailored to the course content
- A full **answer key with worked solutions**

Everything is compiled to PDF via `latexmk` using the `exam` LaTeX class.

## Quick start

1. **Install LaTeX** — MacTeX (macOS), TeX Live (Linux), or MiKTeX (Windows).
2. **Run the skill** — load `SKILL.md` and `template.tex`, then prompt:
   ```
   Create a mock midterm for the lectures in ./course_materials/
   ```
   The agent will ask you to confirm scope, output location, and course details.
   
   Works best in agents that support reading files and executing shell commands (pi, Claude, Codex).
3. The `.tex` source and compiled `.pdf` are dropped into `./Mock_exams/`.

## Requirements

- `latexmk` (part of any standard TeX distribution)
- Standard LaTeX packages (the template declares them all)
- Cross-platform: works on macOS, Linux, and Windows

## Template

`template.tex` is the LaTeX boilerplate — a blank exam you can copy and adapt for any course. It uses the `exam` class with:

- Manual scoring table
- TikZ for diagrams
- `answers`/`noanswers` toggle for the answer key
- Common pitfalls documented inline

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | The skill instructions (works with pi agent, Claude, Codex, etc.) |
| `template.tex` | Blank LaTeX exam template |
| `README.md` | This file |

## License

MIT — see `LICENSE`.
