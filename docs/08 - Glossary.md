# 08 - Glossary

# Trail Framework — Glossary

Terms are listed alphabetically. Where a term has a specific Trail meaning
that differs from common usage, that distinction is called out.

---

## Acceptance Criteria

Testable, verifiable conditions that define when a deliverable is done.
Acceptance Criteria answers "how do we know it's good enough?" — not whether
the work was a success in the real world. That judgment belongs to the
Architect/Product Owner after Trail's job is complete.

## Architect / Product Owner

The human role responsible for defining and owning scope. Produces the intent
package. The only role permitted to define what is being built. May also serve
as Reviewer in smaller projects. Human only — this role cannot be delegated
to AI.

## Artifact

Any file produced or consumed by Trail as part of executing work. Artifacts
are the source of truth. Conversations are not artifacts.

## Assumptions

Decisions the Architect is proceeding on without explicit confirmation.
Documented in `intent.md` to prevent the Manager and Developer from making
different implicit decisions. Assumptions are not ambiguities — they are
choices already made.

## Baseline

A snapshot of the project's known state at the time Trail was adopted. Lives
in `trail/meta/baseline.md`. Not a living document — it does not update with
each run. Changes only when something fundamental shifts (major replatform,
new phase).

## Composite Operating Instructions

The run-level `operating-instructions.md` file, which is assembled by the
Manager from three layers: global operating instructions + intent-level
overrides + run-specific policy. The Developer reads only this file for
operating rules — no need to consult meta or intent-level files directly.

## Constraints

Hard limits the work must operate within. Documented in `intent.md`. Includes
technical, time, budget, policy, platform, and external limits. Vague
constraints are ignored constraints.

## Declaration (Input Declaration)

The act of explicitly listing an input in `intent.md` under Dependencies &
Files. Executors may only access declared inputs — undeclared inputs do not
exist, regardless of whether the executor is aware of them.

## Dependencies & Files

A section of `intent.md` that lists all required input files and external
dependencies. Inputs may live anywhere — inside the Trail folder structure,
on a file share, a network drive, or cloud storage. If it is not declared
here, it does not exist for the purposes of this intent.

## Developer

The role responsible for executing run artifacts and producing output. Human
or AI. Must work only from declared inputs. Must not rely on chat memory or
context outside the run bundle.

## Deliverables

The specific artifacts, documents, files, or outputs that must exist when an
intent is complete. Documented in `intent.md`. Deliverables answers "what do
we hand over?" Acceptance Criteria answers "how do we know it's good enough?"
These are different questions.

## [dev-prompt.md](http://dev-prompt.md/)

The Developer's execution contract. Lists every file the Developer must read,
outputs expected, boundaries, ambiguity handling, and validation steps.
Created by the Manager as part of the run bundle.

## [global-operating-instructions.md](http://global-operating-instructions.md/)

The Trail-wide policy file. Lives in `trail/meta/`. Applies to all intents
and all runs. Defines guardrails for all executors. Stable across the Trail.

## Intent

A bounded, immutable unit of work. Defines what is being built and the
conditions for done. Produced by the Architect/Product Owner. Immutable once
execution begins. New work always requires a new intent — never an edit to
a prior one.

## Intent Package

The three files the Architect/Product Owner produces to define a unit of
work:

- `intent.md` — authoritative scope document
- `manager-instructions.md` — how the Manager operates for this intent
- `operating-instructions-override.md` — intent-level policy overrides

## INTENT_UNUSABLE

A Manager Gate outcome. The Manager cannot produce a runnable bundle because
the intent is structurally unusable — missing required information, contains
internal contradictions, or is ambiguous enough that multiple incompatible
implementations are equally plausible. The Manager stops, documents what is
missing, and does not produce a run bundle.

## [manager-instructions.md](http://manager-instructions.md/)

Defines how the Manager operates for a specific intent — run policy, required
artifacts, naming conventions, quality bar, and output requirements. Part of
the intent package.

## Manager

The role responsible for translating intent into executable run artifacts.
Human or AI. Produces the run bundle. Must not add scope beyond `intent.md`.
Resolves to READY_FOR_DEV or INTENT_UNUSABLE before producing any artifacts.

## Manager Gate

The decision point where the Manager evaluates the intent and resolves to
either READY_FOR_DEV or INTENT_UNUSABLE. No run bundle is produced until
the Manager Gate is passed.

## Meta

The `trail/meta/` folder. The source of truth for the entire Trail. Contains
the baseline, global operating instructions, shared files, and reusable
prompts. Stable across the entire Trail — does not change with each intent
or run.

## Non-Goals (Out of Scope)

Explicit statements of what is excluded from an intent. As important as scope.
If something is obvious but people might assume it is included, it belongs
in Non-Goals.

## [operating-instructions-override.md](http://operating-instructions-override.md/)

Intent-level policy overrides that supplement or replace global operating
instructions for a specific intent. Usually empty. Part of the intent package.
Becomes immutable once execution begins.

## [operating-instructions.md](http://operating-instructions.md/) (run-level)

The composite operating rules file for a specific run. Assembled by the
Manager from global + intent override + run-specific policy. The only
operating rules file the Developer reads.

## READY_FOR_DEV

A Manager Gate outcome. The intent is clear enough to produce atomic,
verifiable tasks. The Manager proceeds to create the run bundle.

## [results.md](http://results.md/)

The Developer's output record. Pre-created by the Manager as an empty
scaffold. Populated by the Developer during execution. Captures: tasks
completed, files changed/added, deviations from plan, assumptions made,
open questions and blockers.

## Reviewer

The human role responsible for verifying output against the intent and run
artifacts. Human only. Outcomes: accept and close the intent, request fixes
(new run), or reject and return to Architect/Product Owner for re-scope
(new intent).

## Run

A timestamped execution container. Lives under `trail/runs/<intent-id>/ run-YYYY-MM-DD-HH-MM-SS/`. Contains the run bundle produced by the Manager
and the output produced by the Developer. Execution-only — answers how, never
what. May fail, pause, or resume.

## Run Bundle

The set of artifacts the Manager produces inside a run folder before the
Developer begins execution. Required: `operating-instructions.md`, `tasks.md`,
`dev-prompt.md`, `start-dev-prompt.md`, `results.md`. Optional: `workplan.md`.

## Scope

What is explicitly included in an intent. Documented in `intent.md`. If
something is not listed in scope, it may be assumed out of scope.

## Specifics

The freeform section of `intent.md` where domain detail lives — UI layout,
data models, workflows, business rules, output formats, content requirements,
etc. Intentionally unstructured. Can be short or long depending on the work.

## [start-dev-prompt.md](http://start-dev-prompt.md/)

The human entry point for invoking the Developer. Contains the exact prompt
the human copies into the Developer agent to begin execution. No policy,
scope, or behavioral rules — invocation only. Required. Created last in the
run bundle.

## [tasks.md](http://tasks.md/)

The ordered, atomic task list the Developer executes against. Each task
includes: task ID, description, inputs, outputs, acceptance criteria. Must
be independently verifiable. Created by the Manager as part of the run bundle.

## Trail

Used two ways:

1. **Trail Framework (TF)** — the methodology itself.
2. **A Trail** — the complete, artifact-driven history of a single project.
Every intent, every run, every decision, from first intent to present.
One project, one repo, one Trail.

## [workplan.md](http://workplan.md/)

An optional Manager artifact for complex runs (10+ tasks or multi-day
execution). Contains: intent summary, assumptions, task overview, risks,
validation strategy. Read by human reviewers only — not by the Developer.