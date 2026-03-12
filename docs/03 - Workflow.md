# 03 - Workflow

Trail executes work as a sequence of explicit, audited handoffs between four roles: Architect/PO, Manager, Developer, and Reviewer. Every transition is artifact-driven — nothing moves forward on conversation alone. Humans approve every stage gate. The diagram below shows the full flow with artifact contracts at each step.

```
Trail Framework (TF) — Workflow with Artifact Contracts

┌────────────────────────────────────────────────────────────────────────┐
│ 0) ARCHITECT / PRODUCT OWNER: Intent Definition                        │
│                                                                        │
│ Intent Package (3 files):                                              │
│ - intent.md                                                            │
│   · Purpose                                                            │
│   · Constraints                                                        │
│   · Dependencies & Files                                               │
│   · Scope (In Scope)                                                   │
│   · Out of Scope (Non-Goals)                                           │
│   · Assumptions                                                        │
│   · Specifics                                                          │
│   · Deliverables                                                       │
│   · Acceptance Criteria                                                │
│   · Notes                                                              │
│                                                                        │
│ - manager-instructions.md                                              │
│   · Run policy, required artifacts, validation rules                   │
│                                                                        │
│ - operating-instructions-override.md                                   │
│   · Intent-level policy overrides (usually empty)                      │
│                                                                        │
└───────────────┬────────────────────────────────────────────────────────┘
                │
                v
┌────────────────────────────────────────────────────────────────────────┐
│ 1) MANAGER (Human or AI): Planning & Decomposition                     │
│                                                                        │
│ Manager Gate — must resolve to one of:                                 │
│   READY_FOR_DEV   → produce run bundle and proceed                     │
│   INTENT_UNUSABLE → list what is missing/contradictory and stop        │
│                                                                        │
│ Run bundle location: trail/runs/<intent>/run-YYYY-MM-DD-HH-MM-SS/     │
│                                                                        │
│ Run bundle (required):                                                 │
│ - operating-instructions.md (composite: global + override + run)       │
│ - tasks.md                                                             │
│ - dev-prompt.md                                                        │
│ - start-dev-prompt.md                                                  │
│ - results.md (empty scaffold)                                          │
│                                                                        │
│ Run bundle (optional):                                                 │
│ - workplan.md (recommended for 10+ task runs)                          │
│                                                                        │
└───────────────┬────────────────────────────────────────────────────────┘
                │
                │  HUMAN CHECKPOINT A
                │  - Review run bundle
                │  - Approve → invoke Developer via start-dev-prompt.md
                │  - Reject → return to Architect for new intent
                v
┌────────────────────────────────────────────────────────────────────────┐
│ 2) DEVELOPER (Human or AI): Implementation                             │
│                                                                        │
│ Reads only: run bundle files                                           │
│                                                                        │
│ Produces:                                                              │
│ - Deliverable (code / files / artifacts)                               │
│ - results.md (populated)                                               │
│   · Tasks Completed (by Task ID)                                       │
│   · Files Changed / Added                                              │
│   · Deviations from Plan (if any)                                      │
│   · Assumptions Made (if any — should be rare)                         │
│   · Open Questions / Blockers                                          │
│                                                                        │
│ Rules:                                                                 │
│ - No new tasks                                                         │
│ - No scope expansion                                                   │
│ - No silent assumptions                                                │
│ - No context outside declared inputs                                   │
│                                                                        │
└───────────────┬────────────────────────────────────────────────────────┘
                │
                v
┌────────────────────────────────────────────────────────────────────────┐
│ 3) REVIEWER (Human): Verification                                      │
│                                                                        │
│ Reviews against:                                                       │
│ - intent.md                                                            │
│ - tasks.md                                                             │
│ - results.md                                                           │
│                                                                        │
│ Outcomes:                                                              │
│ - Accept → close intent                                                │
│ - Request fixes → new run under same intent                            │
│ - Reject → return to Architect for new intent                          │
│                                                                        │
└───────────────┬────────────────────────────────────────────────────────┘
                │
                │  HUMAN CHECKPOINT B
                │  - Accept & close intent
                │  - Request fixes (new run)
                │  - Reject / re-scope (new intent)
                v
┌────────────────────────────────────────────────────────────────────────┐
│ 4) DECISION LOOP                                                       │
│                                                                        │
│ a) Fixes required → Developer (new run, same intent)                   │
│ b) Plan invalid → Manager (new run, same intent)                       │
│ c) Intent unclear or out of scope → Architect (new intent)             │
│ d) Accepted → DONE                                                     │
│                                                                        │
└───────────────┴────────────────────────────────────────────────────────┘

Non-Negotiable Rules
- Missing required artifact sections = invalid artifact
- Invalid artifact blocks progression
- Intent is immutable once Manager gate is passed
- Humans approve all transitions
- All ambiguity must be documented, not inferred
- Developer context must equal file context — no exceptions
```