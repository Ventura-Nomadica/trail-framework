# 01 - Quickstart

# Trail Framework — Quickstart

Your first run in 30 minutes.

This guide gets you from zero to a completed run bundle. It assumes you are
working solo — you are the Architect/Product Owner, you will use AI as the
Manager, and you will use AI as the Developer.

---

## What you are building

A Trail-enabled project has this structure:

```
<project-name>/
└── trail/
    ├── meta/
    │   ├── global-operating-instructions.md
    │   └── baseline.md
    ├── intents/
    │   └── intent-0001/
    │       ├── intent.md
    │       ├── manager-instructions.md
    │       └── operating-instructions-override.md
    └── runs/
        └── intent-0001/
            └── run-YYYY-MM-DD-HH-MM-SS/
                ├── operating-instructions.md
                ├── tasks.md
                ├── dev-prompt.md
                ├── start-dev-prompt.md
                └── results.md
```

Trail lives alongside your project source. It does not touch your code.

---

## Step 1 — Set up your scaffold (5 minutes)

Create the `trail/` folder structure in your project root:

```
trail/
├── meta/
│   └── global-operating-instructions.md   ← copy from Templates → Meta Package
├── intents/
└── runs/
```

If this is an existing project, generate your baseline:

> This is an existing project. Execute the instructions in
`trail/meta/prompts/generate-baseline.md`.
> 

If this is a new project, `baseline.md` may be blank or omitted for now.

---

## Step 2 — Write your intent (15 minutes)

Create `trail/intents/intent-0001/` and copy in the three intent package
templates from Templates → Intent Package.

Fill in `intent.md`. This is the only document in Trail that is 100%
human-authored — take your time here. Friction upfront means less friction
towards the end.

The 10 sections, in order:

1. **Purpose** — what this is and why it exists
2. **Constraints** — tech, time, budget, policy, platform
3. **Dependencies & Files** — required inputs and where to find them
4. **Scope (In Scope)** — what is explicitly included
5. **Out of Scope (Non-Goals)** — what is explicitly excluded
6. **Assumptions** — what you are proceeding on without confirmation
7. **Specifics** — the domain detail (UI, data models, rules, formats, etc.)
8. **Deliverables** — what must exist when this intent is complete
9. **Acceptance Criteria** — testable conditions that define done
10. **Notes** — context, rationale, edge cases worth preserving

`manager-instructions.md` — copy the template as-is. Edit only if you need
to change run policy for this specific intent.

`operating-instructions-override.md` — leave as "None" unless you need
policy different from global for this intent.

---

## Step 3 — Run the Manager (5 minutes)

Open a new chat with your AI of choice. Give it:

1. The contents of `trail/meta/global-operating-instructions.md`
2. The contents of `trail/intents/intent-0001/intent.md`
3. The contents of `trail/intents/intent-0001/manager-instructions.md`
4. The contents of `trail/intents/intent-0001/operating-instructions-override.md`

Then prompt:

> You are acting as the Trail Manager for this intent. Read all provided
files and produce the run bundle per [manager-instructions.md](http://manager-instructions.md/).
> 

The Manager will either:

- Return **READY_FOR_DEV** and produce the run bundle (5 files)
- Return **INTENT_UNUSABLE** and tell you what is missing

If INTENT_UNUSABLE — fix the intent and try again. This is the framework
working correctly, not a failure.

---

## Step 4 — Review the run bundle (5 minutes)

Before invoking the Developer, review:

- `tasks.md` — do the tasks match your intent?
- `operating-instructions.md` — does the policy look right?
- `dev-prompt.md` — are the inputs and outputs correct?

Save all five files into:
`trail/runs/intent-0001/run-YYYY-MM-DD-HH-MM-SS/`

This is Human Checkpoint A. Nothing proceeds without your approval.

---

## Step 5 — Run the Developer

Copy the contents of `start-dev-prompt.md` into a new chat with your
Developer agent (this can be the same AI tool or a different one).

The Developer will execute the tasks and produce:

- The deliverable (code, files, documents, etc.)
- A populated `results.md`

---

## Step 6 — Review and close

Review the output against:

- `intent.md` — does it match what you asked for?
- `tasks.md` — were all tasks completed?
- `results.md` — are deviations and assumptions documented?

This is Human Checkpoint B.

- **Accepted** → the intent is done. Start a new intent for the next piece of work.
- **Fixes needed** → create a new run under the same intent (`run-0002`).
- **Scope is wrong** → create a new intent.

---

## What just happened

You defined work clearly (intent), had it planned into atomic tasks (Manager),
executed it against those tasks (Developer), and verified the output (Reviewer).
Every decision is in a file. Nothing lives in chat.

That is Trail.

---

## What's next

- Read **02 - Roles and Handoffs** to understand role separation in depth
- Read **03 - Manual** for the complete scaffold reference
- Read **04 - Decisions and Invariants** for the non-negotiables
- Check the **FAQ** for common questions