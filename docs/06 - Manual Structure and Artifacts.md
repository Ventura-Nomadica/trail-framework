# 06 - Manual: Structure and Artifacts

# Manual: Structure, Files, and Artifacts

This manual documents the **multi-intent** model as the current operating structure.

If your repository differs, treat `meta/` as the source of truth and align structure over time.

---

## Top-level structure (current model)

All Trail files live under a single `trail/` folder at the project root. This keeps Trail isolated from your project source and easy to exclude from builds or ignore in version control.

At minimum, a Trail-enabled project has:

- `trail/meta/` (authoritative)
- `trail/intents/` (immutable work units, one folder per intent)
- `trail/runs/` (execution containers, grouped by intent)

---

## `trail/meta/` (source of truth)

Purpose: define the Trail-wide reality, identity, and global operating constraints. `trail/meta/` is stable across the entire Trail — it does not change with each intent or run.

### `trail/meta/trail.md`

- The product's persistent identity document.
- Describes what the product is, who it's for, why it exists, permanent constraints, technology and architecture decisions, and product principles.
- Required reading for the Manager on every run.
- Changes rarely — only when product identity, audience, or permanent constraints shift.
- Not read directly by the Developer; the Manager surfaces relevant context into run artifacts.

`trail.md` is distinct from `baseline.md`: trail is *identity and purpose* (what are we building and why), baseline is *technical state* (what exists on the ground right now).

### `trail/meta/baseline.md`

- Created once, when Trail is adopted on a project.
- A snapshot of the project's known state at that point in time — what exists, what the tech stack is, what must not be broken.
- Anchors reality so executors don't invent it.
- **Not a living document.** It does not get updated with each run. `results.md` handles run-level changes.
- Changes rarely and deliberately — only when something fundamental shifts (major replatform, new phase). Never automatically.
- May be blank in a brand-new project with nothing yet to document.

> Many AI development tools generate an equivalent file automatically (Claude Code, Codex, and others produce project context files for the same purpose). If your tool generates one, it can serve as or directly inform your `meta/baseline.md`.
> 

**Generating [baseline.md](http://baseline.md)**

Trail ships a prompt for this: `trail/meta/prompts/generate-baseline.md`.

To generate your baseline, point any AI development tool at your project and run:

> This is an existing project. Execute the instructions in `trail/meta/prompts/generate-baseline.md`.
> 

The prompt will analyze your project and produce `trail/meta/baseline.md`. It is read-only — it will not modify any existing files.

### `trail/meta/files/`

Shared project input pool, available to all intents. Place here: icons, images, brand assets, global documents, copy, help text, datasets, and any other inputs that span multiple intents.

### `trail/meta/prompts/`

Reusable prompt files for common Trail operations. These are not intents — they are standalone instructions you trigger directly against an AI tool.

Trail ships one prompt: `generate-baseline.md`. Copy it into `trail/meta/prompts/` when setting up your scaffold.

### `trail/meta/global-operating-instructions.md`

- Applies to all intents and all runs.
- Defines guardrails (authority, precedence, scope discipline, ambiguity handling, logging expectations, failure behavior).
- Stable across the Trail.
- Policy, scope rules, and behavioral constraints only (not task-specific).

> Effective operating instructions are the sum of:
> 

> `trail/meta/global-operating-instructions.md` + intent-level `operating-instructions-override.md` + run-specific policy
> 

---

## `trail/intents/` (immutable units of work)

### Folder naming

Two acceptable conventions:

- `trail/intents/intent-0001/` (sequence-based)
- `trail/intents/intent-YYYY-MM-DD-mm-ss/` (timestamp-based)

Either works as long as ordering is unambiguous and intent folders are never modified once execution begins.

### Intent Package Structure

Each intent folder contains three files that together form the "intent package":

### `trail/intents/<intent>/files/` (recommended)

Intent-scoped input pool. Place here: any files specific to this intent (mockups, data exports, domain docs, etc.).

Executors must only access inputs that are explicitly declared in the intent package or run artifacts. Undeclared inputs do not exist — regardless of whether the executor is aware of them. If an input lives outside the Trail folder structure, it must be declared in `intent.md` under Dependencies & Files with sufficient information for the executor to locate it.

**Default lookup order for inputs inside the Trail folder structure:**

1. `trail/intents/<intent>/files/`
2. `trail/meta/files/`

**No run-level `files/` folder exists.** Inputs live at the intent or meta level only.

---

### `intent.md` (authoritative scope)

The Architect-owned document that defines a single unit of work. It is the only document in Trail that is 100% human-authored.

Sections (in order):

- **Purpose** — what this is and why it exists
- **Constraints** — tech, time, budget, policy, platform, external limits
- **Dependencies & Files** — required input files and external dependencies. Inputs may live anywhere — inside the Trail folder structure, on a file share, a network drive, or cloud storage. Location does not matter. Declaration does. Anything not declared here does not exist for the purposes of this intent.
- **Scope (In Scope)** — what is explicitly included
- **Out of Scope (Non-Goals)** — what is explicitly excluded
- **Assumptions** — what the Architect is proceeding on without confirmation
- **Specifics** — freeform domain detail (UI, data models, workflows, business rules, formats, etc.)
- **Deliverables** — the specific artifacts or outputs that must exist when the intent is complete
- **Acceptance Criteria** — testable conditions that define done (not success — done)
- **Notes / Ambiguities** — unresolved questions and context worth preserving

Intent rules:

- Intent is Architect-owned.
- Intent is immutable once execution begins.
- New work = new intent.
- Old intents are never deleted.

`intent.md` inherits persistent product context from `trail/meta/trail.md`. The intent does not need to re-describe the product — it focuses on the specific unit of work.

### `manager-instructions.md`

Defines how the Manager operates for this specific intent:

- Run folder structure and naming
- Required artifacts the Manager must create
- Artifact validation rules
- Output requirements

This is the operational contract for the Manager role.

### `operating-instructions-override.md`

- Intent-level policy overrides that supplement or replace global operating instructions.
- Usually empty; only exists if this intent needs different policy than global.
- Contains policy, scope rules, and behavioral constraints only — not tasks or specific work.
- Becomes immutable once execution begins.

---

## `trail/runs/` (execution containers)

Purpose: store the execution artifacts and results produced while completing an intent.

Structure:

- `trail/runs/<intent-id>/run-YYYY-MM-DD-HH-MM-SS/`

Run folder naming uses timestamps: ordering is unambiguous, human-readable, and collision-free without any naming decisions.

Runs are:

- decided by the Manager (count, sequence)
- allowed to fail/pause/resume
- execution-only (they answer *how*, never *what*)

Runs consume:

- `trail/meta/trail.md` (Manager reads; not passed directly to Developer)
- `trail/meta/baseline.md`
- `trail/meta/global-operating-instructions.md`
- `trail/intents/<intent>/intent.md`
- `trail/intents/<intent>/manager-instructions.md`
- `trail/intents/<intent>/operating-instructions-override.md`
- run artifacts created by the Manager

### Files inside a run folder (Manager creates these)

### `operating-instructions.md` (composite)

- **Composite** of:
    1. Global operating instructions (from `trail/meta/global-operating-instructions.md`)
    2. Intent-level overrides (from `operating-instructions-override.md`, if present)
    3. Run-specific policy/rules (defined by Manager for this run)
- Contains policy, scope rules, and behavioral constraints only.
- Executor reads ONLY this file for operating rules (fully composed, no need to read meta or intent level).

### `tasks.md`

- The execution task list.
- Each task must have: task ID, description, inputs, outputs, acceptance criteria.
- Must be atomic and independently verifiable.

### `dev-prompt.md`

- The Developer's entry point and execution contract.
- Must define: files to read, outputs expected, boundaries, ambiguity handling, validation steps.

### `start-dev-prompt.md`

- The human-run entry point for invoking the Developer.
- Contains the exact prompt the human copies into the Developer agent.
- No policy, scope, or behavioral rules — just the invocation.
- Required. Created last in the run bundle.

### `results.md`

- Pre-created by the Manager as an empty scaffold.
- Populated by the Developer during execution.
- Must capture: tasks completed, files changed/added, deviations, assumptions made, open questions/blockers.

### `workplan.md` (optional)

- Manager's planning artifact for complex runs.
- Use for 10+ tasks or multi-day execution. Skip for simple runs.
- Contains: intent summary, assumptions, task overview, risks, validation strategy.
- Not read by Developer — for human reviewer only.

---

## Baseline (onboarding and start points)

`meta/baseline.md` is a snapshot, not a living document. It captures the known state of the project when Trail was adopted — what exists, what matters, what must not be broken. It does not get updated with each run.

Baseline should answer:

- what exists today
- what the tech stack is
- where the important files are
- what must not be broken

Use `meta/prompts/generate-baseline.md` to generate it. Trail does not ship a blank baseline template — your project's reality is better captured by analysis than by filling in a form.

---

## Review loop

A run is not "done" when code compiles.

A run is done when:

- tasks and acceptance criteria are satisfied
- the Reviewer verifies the output against intent and run artifacts
- and the Architect/PO decides whether to close the intent or create a new one

If fixes are required:

- create another run under the same intent (preferred)

If scope is wrong:

- create a new intent (preferred)