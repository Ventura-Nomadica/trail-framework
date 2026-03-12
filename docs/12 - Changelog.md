# 12 - Changelog

# Changelog (Superseded Models & Major Changes)

This changelog exists to keep documentation honest without turning the manual into a history book.

## Change: AI-instructions → Operating-instructions (February 2026)

**What changed**

- `meta/ai-instructions.md` → `meta/global-operating-instructions.md`
- Intent-level `ai-instructions.md` → `operating-instructions-override.md`
- Run-level `ai-instructions.md` → `operating-instructions.md` (composite)

**Why**

- "AI-instructions" implied the files were only for AI executors, but Trail works with human executors too.
- "Operating-instructions" accurately describes what these files are: policy, scope rules, and behavioral constraints for whoever is executing.
- Clarifies that run-level operating-instructions are a composite (global + intent override + run-specific).

**Intent Package Structure**

- Each intent now explicitly includes three files:
    - `intent.md` (what we're accomplishing)
    - `manager-instructions.md` (how Manager operates)
    - `operating-instructions-override.md` (intent-level policy overrides)

**Impact**

- More accurate terminology that reflects Trail's human + AI design.
- Clearer composition model: run-level file is fully composed, executor reads only one file.
- Intent package structure is now explicit and complete.

## Change: Single-intent → Multi-intent (current model)

**What changed**
- Trail moved from “one intent per project” to “a product is a sequence of intents.”

**Why**
- One-off builds work fine as single-intent.
- Long-lived products require preserved chapters and bounded work units.

**Impact**
- Introduced explicit top-level `intents/` folder (one folder per immutable intent).
- `runs/` are now grouped under each intent.

## Change: Run location model (older → current)

**Older model observed in notes**
- Runs created under a single location like:
- `adf/runs/active/`
- with timestamp-named run folders (`run-YYYY-MM-DD-HH-MM-SS`)

**Current model**
- Runs live under the intent:
- `runs/<intent-id>/run-01/`, `run-02/`, etc.

**Status**
- Older model is superseded by the multi-intent “Final Model.”

## Change: Manager artifacts (variations)

Two closely-related run artifact sets appear:

1. **Manager bundle (current)**
- `operating-instructions.md` (composite: global + intent override + run-specific)
- `dev-prompt.md`
- `tasks.md`
1. **Workflow diagram artifact set (observed in notes)**
- `workplan.md`
- `tasks.md`
- `results.md`
- `review.md`

**Status**
- The Manager bundle is canonical for run initiation.
- The workflow set is still useful as an optional operational standard.
- See `06_Contradictions.md` for how to handle this safely.

## Change: Files locations and missing input policy (February 2026)

**What changed**

- Introduced `meta/files/` as the shared project input pool.
- Introduced `intents/<intent>/files/` as the recommended intent-scoped input location.
- Defined explicit lookup order: intent files → meta files.
- Established missing input policy: non-critical = generate placeholder + record; critical = stop and report.
- Confirmed: no run-level `files/` folder.

**Why**

- Prior docs were silent on where input files (icons, docs, datasets) should live, leading to inconsistent placement and executor confusion.
- Deterministic lookup order eliminates ambiguity without requiring executors to search.

---

## Change: Naming (ADF → TDF / Trail)

Trail Development Framework (TDF) is the current name.
Older documents may refer to “ADF” (AI Dev Framework).

---

## Change: Documentation updates (March 2026)

**Licensing**

- Added CC BY 4.0 license declaration to 00 - README and 01 - Philosophy.
- Copyright © 2026 Ventura Nomadica.
- MIT license planned for future code components (templates, CLI tools). Not yet applicable.

**Versioning policy**

- Added versioning stance to 04 - Decisions and Invariants.
- Versioning is a human responsibility. Trail does not enforce it.
- `version.md` in `meta/` documented as a recommendation only. No template shipped. No format specified.

**Trail's position on external tools and methodologies**

- Added "What Trail is not" section to 00 - README.
- Strengthened existing disclaimer in 01 - Philosophy into an explicit stance.
- Trail has no opinion on repositories, project management tools, or development methodologies.
- Trail's opinions are limited to: artifacts, roles, and handoffs.
- Companion guides (Trail + Git, Trail + Agile, etc.) are planned but not part of v1.0.

**Baseline clarification and prompts scaffold**

- `meta/baseline.md` clarified as a snapshot, not a living document. Does not update with each run.
- Introduced `meta/prompts/` as a scaffold location for reusable prompt files.
- Trail now ships `generate-baseline.md` in `meta/prompts/` — a read-only codebase analysis prompt that produces `meta/baseline.md`.
- Templates index updated to reference `generate-baseline.md` and the Prompts category.
- 03 - Manual updated throughout to reflect all of the above.

---

## Change: Documentation cleanup and Trail Framework reorganization (March 2026)

**Docs renamed to Trail v1.5**

- "Trail v1.4 Docs" renamed to "Trail v1.5 Docs" to reflect the scope of changes made in this session.

**"What is a Trail?" — new core concept**

- Defined "Trail" as a noun: the complete, artifact-driven history of a single project.
- Rule established: one project, one repo, one Trail.
- Cross-project dependencies are declared in `intent.md`, never merged into the Trail.
- Added to 01 - Philosophy as a dedicated section.
- Updated `meta/` description in 03 - Manual to "stable across the Trail."

**Workflow page updated**

- Rewrote the Workflow page (previously ADF-era ASCII diagram) to reflect current Trail v1.0 roles, artifact sets, and handoff sequence.
- Updated terminology throughout: Architect/PO, Manager, Developer, Reviewer (Human).
- Manager Gate outcomes (READY_FOR_DEV / INTENT_UNUSABLE) added to the diagram.
- Added introductory paragraph.

**Folder Structure page updated**

- Rewrote the Folder Structure page (previously ADF-era) to reflect current Trail scaffold.
- Updated `meta/` comment to "stable across the Trail."
- Removed all superseded ADF paths (scratch, tools, active/archive run model).

[**README.md](http://README.md) updated**

- Rewrote [README.md](http://README.md) to reflect current Trail v1.0 model.
- Added "What Trail is Not" section.
- Added "A Final Note" (salvaged from original ADF README).
- Archived the original ADF-era [README.md](http://README.md).

**Run artifact header requirement**

- Established that all Trail run artifacts must include a standard header: `Run:` and `Date:`.
- Header is the Manager's responsibility at run creation time — not added to templates.
- `manager-instructions.md` (v2) already reflects this requirement.

**ADF pages archived**

- The following ADF-era pages were moved to Archive as superseded:
    - ADF in 60 Seconds
    - ADF - Next Steps & Positioning
    - ADF - One-Page Framework Roadmap
    - ADF - Run Initiation & Role Handoff
    - ADF - Intents, Meta, and Runs (Final Model)
    - [README.md](http://README.md) (original ADF version)

**06 - Contradictions**

- All three contradictions identified as fully resolved.
- Open questions identified as answered in current docs.
- Full update to be completed in a future session.

---

## Change: [intent.md](http://intent.md) formalized (March 2026)

**What changed**

- `intent.md` template fully redesigned with a standardized 10-section structure.
- Each section includes inline instructions so the template is self-describing — no need to reference external docs while writing an intent.
- Status field removed from header (noise until immutability is enforced by Trail rules, not a file field).
- Owner and Last Updated retained in header.

**New section structure (in order)**

1. Purpose
2. Constraints
3. Dependencies & Files (replaces "Files" — now explicitly includes external dependencies)
4. Scope (In Scope)
5. Out of Scope (Non-Goals)
6. Assumptions (new — makes implicit decisions explicit)
7. Specifics (freeform domain detail — intentionally unstructured)
8. Deliverables (new — names what must exist at the end, separate from acceptance criteria)
9. Acceptance Criteria (defines done, not success)
10. Notes

**Key decisions**

- Purpose and Objective merged into Purpose. Objective is implied — deliver the purpose within the constraints.
- Deliverables and Acceptance Criteria are separate sections. Deliverables answers "what do we hand over?" Acceptance Criteria answers "how do we know it's good enough?" These are genuinely different questions, especially in non-software work.
- Acceptance Criteria defines done, not success. Whether the work succeeds in the real world is a human judgment that happens after Trail's job is complete.
- Assumptions is a new required section. Assumptions are not ambiguities — they are decisions already made implicitly. Making them explicit prevents the Manager and Developer from making different implicit decisions.
- Specifics is intentionally freeform. No required structure — format should match the work.

**Docs updated**

- `intent.md` template replaced in Templates → Intent Package.
- 03 - Manual updated to reflect new section structure.
- 02 - Roles and Handoffs updated to reflect new [intent.md](http://intent.md) description.

---

## Change: Trail scaffold, run naming, and run artifacts formalized (March 2026)

**What changed**

- All Trail files now live under a single `trail/` folder at the project root. This isolates Trail from project source and makes it trivial to exclude from builds or version control ignores.
- Run naming formalized as `run-YYYY-MM-DD-HH-MM-SS`. Timestamp format provides ordering, human readability, and collision avoidance without any naming decisions. Sequential naming (`run-01`) retired.
- Run header fields updated: `Run:` and `Purpose:` only. `Date:` removed — the folder name carries the timestamp.
- `start-dev-prompt.md` formalized as a required run artifact. It is the exact prompt the human copies into the Developer agent to begin execution. Contains no policy or scope — invocation only.
- `results.md` formalized as a required run artifact, pre-created by the Manager as an empty scaffold for the Developer to populate.

**Required run artifacts (current)**

1. `operating-instructions.md` (composite)
2. `tasks.md`
3. `dev-prompt.md`
4. `start-dev-prompt.md`
5. `results.md`

Optional: `workplan.md` (for 10+ task runs)

**Docs updated**

- 03 - Manual: scaffold structure, all paths updated to `trail/`, run naming, run artifacts list
- 02 - Roles and Handoffs: Manager artifacts list updated, paths updated
- 04 - Decisions and Invariants: run naming convention updated, paths updated, ADF-era strikethrough text cleaned up

---

## Change: Documentation structure, navigation, and new docs (March 2026)

**What changed**

- Documentation reordered for developer audience. New sequence prioritizes action (Quickstart) over reference (Manual), and visual orientation (Workflow, Folder Structure) before deep reading.
- Three new documents added: Quickstart, Glossary, FAQ.
- README updated to reflect new document order, correct `trail/` paths throughout, and updated file declaration language.
- All documents renumbered to match new sequence.

**New document order**

1. Quickstart
2. Philosophy & Operating Model
3. Workflow
4. Roles and Handoffs
5. Folder Structure
6. Manual: Structure and Artifacts
7. Decisions and Invariants
8. Glossary
9. FAQ
10. Templates
11. Contradictions
12. Changelog

**Rationale**

- Quickstart first: developers want to try before they commit to reading everything.
- Workflow and Folder Structure before Manual: visual context before deep reference.
- Glossary and FAQ after core docs: support material, not learning material.
- Templates at the back: grab-and-go once you understand the system.

[**Global-operating-instructions.md](http://Global-operating-instructions.md) formalized**

- Updated from ADF [ai-instructions.md](http://ai-instructions.md) to Trail terminology throughout.
- Roles section added: all four roles defined with human/AI constraints explicit.
- File Access Boundaries rewritten: constraint is on undeclared access, not location. Inputs may live anywhere — declaration is what matters.
- No-questions rule added to Assumptions section with rationale: questions move work into chat, chat is not the record.

---

## Change: Split licensing model adopted (March 2026)

**What changed**

- Trail Framework now uses a split licensing model:
    - **Documentation** (philosophy, methodology, reference materials) — CC BY 4.0 (unchanged)
    - **Scaffold** (folder structure and template files in the downloadable zip / GitHub repo) — MIT License (new)
- `LICENSE` file added to the scaffold root. MIT License, Copyright © 2026 Ventura Nomadica.
- `LICENSE` added as a subpage of 10 - Templates (canonical scaffold reference).

**Why**

- CC BY 4.0 requires attribution in derivative works. For documentation that people read and cite, that's appropriate. For scaffold files that get copied into business projects, it creates friction that blocks enterprise adoption.
- MIT is the de facto standard for "use this freely in your commercial project." No attribution required in work product — only the LICENSE file itself must be preserved, which stays in the scaffold.

**Pages updated**

- Framework Licensing — rewritten to document both licenses clearly
- 00 - README — License section updated to reflect split model
- 05 - Folder Structure — `LICENSE` added to repo root in folder tree
- LICENSE (Meta Package template) — updated from CC BY 4.0 to MIT
- Docs updated: 04 - Decisions and Invariants invariants 6 and 7 updated to match.