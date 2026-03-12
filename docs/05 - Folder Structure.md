# 05 - Folder Structure

The Trail lives alongside your project source. One project, one repo, one Trail.

```
The Trail lives alongside your project source. One project, one repo, one Trail.

<project-name>/
├── trail/                                              # All Trail files live here — isolated from project source
│   ├── meta/                                          # Source of truth, stable across the Trail
│   │   ├── baseline.md                                # Project snapshot (generated, not hand-written)
│   │   ├── global-operating-instructions.md           # Global policy for all executors
│   │   ├── files/                                     # Shared inputs available to all intents
│   │   └── prompts/                                   # Reusable prompt files
│   │       └── generate-baseline.md                   # Generates meta/baseline.md
│   │
│   ├── intents/                                       # Immutable units of work
│   │   └── intent-0001/                               # One folder per intent (never modified after execution begins)
│   │       ├── intent.md                              # Purpose, constraints, scope, specifics, deliverables, acceptance criteria
│   │       ├── manager-instructions.md                # How Manager operates for this intent
│   │       ├── operating-instructions-override.md     # Intent-level policy overrides (usually empty)
│   │       └── files/                                 # Intent-scoped inputs (recommended default)
│   │
│   └── runs/                                          # Execution containers, grouped by intent
│       └── intent-0001/
│           ├── run-2026-03-01-09-00-00/               # Timestamped run folder
│           │   ├── operating-instructions.md          # Composite: global + override + run-specific
│           │   ├── tasks.md                           # Ordered, atomic task list
│           │   ├── dev-prompt.md                      # Developer execution contract
│           │   ├── start-dev-prompt.md                # Human entry point — copy/paste to invoke Developer
│           │   ├── results.md                         # Developer output record
│           │   └── workplan.md                        # Optional: complex runs (10+ tasks)
│           └── run-2026-03-01-14-30-00/               # Subsequent run if fixes required
│               └── ...
│
├── src/                                               # Your project source (Trail-agnostic)
├── tests/
├── docs/
├── LICENSE                                            # MIT License — covers the Trail scaffold
└── README.md
```