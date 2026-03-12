# Roles & Handoffs

Trail uses **four roles** (with some environments treating Reviewer as a "3.5th" role because it can be the Architect or Product Owner).

## Role 1: Architect / Product Owner (Human)

**Primary responsibility:** define and own the *what*.

Produces:

- `trail/meta/trail.md` (product identity, permanent constraints, product principles — created once, updated rarely)
- Intent package:
  - `intent.md` (the authoritative scope document — purpose, constraints, dependencies, scope, assumptions, specifics, deliverables, acceptance criteria, notes)
  - `manager-instructions.md` (how Manager operates for this intent)
  - `operating-instructions-override.md` (intent-level policy overrides, if needed)
- Updates to `trail/meta/` when reality changes (baseline, global instructions, trail identity)
- Files to be used for the work (images, icons, documents, datasets, etc.)

Key rule:

- The Architect/PO is the only role allowed to define scope.

## Role 2: Manager (Human or AI)

**Primary responsibility:** translate intent into **execution artifacts**.

Produces a run bundle inside a timestamped run folder (`trail/runs/<intent-id>/run-YYYY-MM-DD-HH-MM-SS/`):

**Required:**

- `operating-instructions.md` (composite of global + intent override + run-specific policy)
- `tasks.md` (ordered, atomic work items)
- `dev-prompt.md` (execution contract)
- `start-dev-prompt.md` (human entry point for invoking the Developer)
- `results.md` (empty scaffold for Developer to populate)

**Optional:**

- `workplan.md` (for complex runs with 10+ tasks)

Manager rules:

- Must read `trail/meta/trail.md` and `intent.md` before producing run artifacts. The Developer reads neither of these files directly.
- Must distill all context the Developer needs — product identity, scope, constraints, technology decisions, file paths — into the run artifacts. Nothing relevant to execution may remain only in intent or meta files.
- Must restate intent, not reinterpret it.
- Must surface assumptions explicitly.
- Must define validation strategy (how we know it's done).
- Must not add new scope.
- Must create the run bundle per `manager-instructions.md`.
- Must compose `operating-instructions.md` as a composite of: global operating instructions + intent-level override + run-specific policy.
- Must ensure all required input files are explicitly listed in `tasks.md` or `dev-prompt.md` by exact path.
- Must enforce the missing input policy: if a non-critical input is absent, create a reasonable placeholder and record it in `results.md`. If a critical input is absent, do not invent it — stop and report the missing dependency.

## Role 3: Developer (Human or AI)

**Primary responsibility:** execute the run artifacts and produce output.

Developer rules:

- Must work **only** from run files: `operating-instructions.md`, `tasks.md`, `dev-prompt.md`, and any input files explicitly listed in those artifacts.
- Must **not** read `intent.md`, `trail.md`, `global-operating-instructions.md`, or any other intent-level or meta-level file. The Manager is responsible for surfacing all necessary context into the run artifacts.
- Must not rely on chat memory or "earlier context" that isn't written down.
- Must not invent tasks or expand scope.
- Must record deviations and assumptions in `results.md`.

The Developer's permitted file access:

- **Run files** (always): the contents of the current run folder
- **Input files** (when explicitly listed in run artifacts): `trail/intents/<intent>/files/` and `trail/meta/files/` subfolders only

Output:

- code / files / diffs (the deliverable)
- `results.md` populated with tasks completed, files changed, deviations, assumptions, open questions

## Role 4: Reviewer (Human)

**Primary responsibility:** verify and challenge.

Reviewer evaluates against:

- the intent definition
- the run tasks
- the produced output

Reviewer outcomes:

- Accept and close the intent
- Request fixes (new run within the same intent)
- Reject and return to Architect/PO for re-scope (new intent)

### "3.5 roles" reality

In practice:

- Architect and Product Owner are often the same human.
- Reviewer may be the same human as Architect/PO in small projects.
- In larger environments, Reviewer is separate to enforce independence.

Trail supports both; the invariant is that **someone must perform the review step** before the intent is considered complete.

## Handoff sequence

1. Architect/PO produces an intent package (and maintains `trail/meta/trail.md`).
2. Manager reads `trail.md` + intent package, then produces the run bundle — surfacing all execution-relevant context into the run artifacts.
3. Developer executes **from run files only** and produces output + results.
4. Reviewer verifies and returns decision back to Architect/PO.

## Hard constraint for execution

When Manager and Developer are AI or human:

- The Developer must not depend on any "memory" outside the written artifacts.
- Whether it is a separate chat is optional, but the constraint is mandatory:

**Developer context must equal file context.**

This is one of Trail's highest-leverage controls against drift and hallucination.