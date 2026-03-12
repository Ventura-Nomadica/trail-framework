# 00 - README

# Trail Framework (TF) — Documentation Pack

TF (usually just "Trail" or "Trail Framework") is an **artifact-driven development operating system** for building with **humans and AI as first-class actors** — without letting "the conversation" become the source of truth.

Trail's core bet is simple:

- **Conversations drift. Files don't.**
- AI can execute well, but it's a weak decision-maker and a worse historian.
- Reliability comes from **role separation + explicit artifacts + mechanical handoffs**.

---

## Documents in this pack

1. **Quickstart** — your first run in 30 minutes
2. **Philosophy & Operating Model** — what Trail is and why it exists
3. **Workflow** — the full flow in one view
4. **Roles & Handoffs** — who does what and when
5. **Folder Structure** — where everything lives
6. **Manual: Structure and Artifacts** — deep reference for the scaffold
7. **Decisions & Invariants** — the non-negotiables
8. **Glossary** — definitions for Trail-specific terms
9. **FAQ** — common questions and edge cases
10. **Templates** — ready-to-copy starting points
11. **Contradictions** — known unreconciled notes, historical record
12. **Changelog** — superseded models and major changes

---

## Design outcomes Trail is optimizing for

- **Intent integrity:** execution stays aligned to what a human decided.
- **Auditability:** a future human can reconstruct *why* and *how* something was built.
- **Low hallucination surface area:** assumptions are documented, not buried in chat.
- **Composable work:** long-lived products evolve through **multiple intents**.

---

## What Trail is not

Trail is not a repository, a project management tool, a project methodology, or a replacement for your existing workflow tooling.

Trail has no opinion on whether you use Git, SVN, or a shared folder. It has no opinion on whether you run Agile, Shape Up, Scrum, or nothing. It does not replace Jira, Linear, Notion, or any coordination layer you already have.

Trail's opinions are strictly limited to: artifacts, roles, and handoffs.

Bring your own tools. Trail sits above them.

Companion guides for specific integrations (Trail + Git, Trail + Agile, etc.) are planned but are not part of v1.0.

---

## Where to put files

Trail distinguishes **files** (inputs consumed during work) from **artifacts** (outputs produced by a run).

### Meta files (project reality anchor)

The `trail/meta/` folder contains the files that define the project's persistent identity, state, and operating rules. Together they form the **project reality anchor**:

- `trail/meta/trail.md` — the product's persistent identity: what it is, who it's for, why it exists, permanent constraints, and product principles. Required reading for the Manager on every run. Changes rarely (only when product identity shifts).
- `trail/meta/baseline.md` — the current known technical state of the system. What exists today, what's deployed, what must not be broken. Changes per phase.
- `trail/meta/global-operating-instructions.md` — policy, scope rules, and behavioral constraints for all executors. Stable across the project.
- `trail/meta/files/` — shared project inputs available to all intents (icons, brand assets, global docs, datasets, etc.)

### Input file locations

Two `files/` locations exist:

- `trail/meta/files/` — shared project inputs available to all intents (icons, brand assets, global docs, datasets, etc.)
- `trail/intents/<intent>/files/` — intent-scoped inputs (recommended for anything specific to a single intent)

Inputs may live outside the Trail folder structure entirely — on a file share, network drive, or cloud storage. What matters is declaration, not location. Anything not declared in `intent.md` does not exist for the purposes of that intent.

**Default lookup order for inputs inside the Trail folder structure:**

1. `trail/intents/<intent>/files/`
2. `trail/meta/files/`

**No run-level `files/` folder exists.** Inputs live at the intent or meta level only.

---

## License

Trail Framework uses a split licensing model.

**Documentation** (philosophy, methodology, reference materials) — [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)

**Scaffold** (folder structure and template files in the downloadable zip / GitHub repo) — [MIT License](10%20-%20Templates/LICENSE%2031a67a11cf2381eda5e3c6e6c443eff0.md)

The scaffold is MIT-licensed so you can use it freely in personal or commercial projects without attribution requirements in your work product.

Copyright © 2026 Ventura Nomadica. See [Framework Licensing](https://www.notion.so/Framework-Licensing-31567a11cf23802383dce7a88638e3d7?pvs=21) for full terms.