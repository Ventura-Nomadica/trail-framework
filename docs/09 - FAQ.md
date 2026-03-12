# 09 - FAQ

# Trail Framework — FAQ

---

## Getting started

**Do I need to use all four roles?**
Not all at once, but all four must be performed. In a solo project, you are
the Architect/Product Owner and the Reviewer. AI handles Manager and Developer.
The roles still exist — they just collapse onto fewer people.

**Can I skip the Manager if I know what tasks need doing?**
No. The Manager Gate exists to validate the intent before execution begins.
Skipping it means skipping the only checkpoint that catches unusable intents
before the Developer wastes time on them. If the Manager is fast (simple
intent, clear tasks), that is fine — but the step must happen.

**Do I have to use AI? Can Trail be used with human executors only?**
Yes. Trail is designed for humans, AI, or mixed teams. The framework does not
care who fills the Manager or Developer role — it cares that the role contract
is followed.

**Can I use Trail for non-software work?**
Yes. Trail was designed to be domain-agnostic. It has been used for app
development, Chrome extensions, and business document packages. Any complex
knowledge work with clear deliverables and acceptance criteria is a candidate.

---

## Writing intents

**What is the difference between Constraints and Assumptions?**
Constraints are hard limits imposed from outside — "must run on iOS," "must
cost under $500," "must comply with GDPR." Assumptions are decisions you have
made internally without explicit confirmation — "assumes the dev environment
already has Flutter installed," "assumes Canadian employment law applies."
Constraints bound the work. Assumptions declare what you are proceeding on.

**What is the difference between Deliverables and Acceptance Criteria?**
Deliverables answers "what must exist?" — the app build, the four HR
documents, the Chrome extension zip. Acceptance Criteria answers "how do we
know it is good enough?" — the app runs on iOS and Android, the documents are
cross-referential, the extension exports to a correctly formatted Markdown
file. They are different questions and both matter.

**What goes in Specifics?**
Anything that makes this intent unique — UI layout, data models, workflows,
business rules, output formats, interaction patterns, content requirements.
There is no required structure. A Chrome extension intent might have an output
format specification. A Flutter app intent might have screen layouts and
data schemas. An HR document intent might have jurisdiction requirements.
Format to match the work.

**How long should an intent be?**
As long as it needs to be. A simple one-off script might have a one-paragraph
Specifics section. A full mobile app might have ten subsections. The test is:
can the Manager produce atomic, verifiable tasks from this intent without
guessing? If yes, it is long enough.

**What if I don't know all the details yet?**
Write what you know. Use the Assumptions section to declare what you are
proceeding on. Use Notes to flag what is still uncertain. If the Manager
returns INTENT_UNUSABLE, it will tell you exactly what is missing. That is
the framework doing its job.

---

## Running the Manager

**What does INTENT_UNUSABLE actually mean?**
It means the Manager cannot produce atomic, verifiable tasks without guessing.
It is only valid when the intent is missing required information, contains
internal contradictions, or is ambiguous enough that multiple incompatible
implementations are equally plausible. It is not a judgment on the quality of
the idea — it is a structural check on whether the intent is executable.

**What if the Manager adds scope I didn't ask for?**
That is a violation of the Manager contract. The Manager must restate intent,
not reinterpret it. Review `tasks.md` before invoking the Developer. If tasks
exceed the scope in `intent.md`, send the Manager back. A new run is cheaper
than rework.

**Can the Manager decide how many runs are needed?**
Yes. The Manager decides the number and sequence of runs. For simple work,
one run is usually sufficient. For complex work, the Manager may plan multiple
runs with different objectives. This is part of the Manager's planning role.

---

## During execution

**What if the Developer hits something unexpected mid-run?**
The Developer documents it in `results.md` and stops if unable to proceed
without violating operating rules or inventing scope. Silent failure is not
acceptable. A stopped run with clear documentation is a better outcome than
a completed run with hidden assumptions.

**Can the Developer ask clarifying questions?**
No. Questions move work into chat — chat is not the record. If ambiguity
exists, the Developer chooses the simplest interpretation consistent with
stated intent, documents the decision in `results.md`, and continues. If
genuinely blocked, the Developer stops and documents why.

**Can I change the intent mid-run?**
No. The intent is immutable once the Manager Gate is passed. If you need to
change scope, the run must stop and a new intent must be written. This is
intentional — it prevents scope creep from fragmenting across chat history
and artifacts.

---

## After execution

**When does a fix become a new run vs. a new intent?**
A new run if the fix is within the existing scope — the output was wrong but
the intent was right. A new intent if the scope needs to change — what you
asked for turned out to be the wrong thing to ask for. When in doubt: if the
Acceptance Criteria in the original intent still apply, it is a new run.
If you need to change the Acceptance Criteria, it is a new intent.

**Do old intents and runs get deleted?**
Never. Old intents are preserved. Old runs are preserved. The Trail is the
complete history of the project — deleting any part of it breaks auditability.

**What does "done" mean in Trail?**
Done means the Acceptance Criteria in `intent.md` are satisfied and the
Reviewer has accepted the output. Done does not mean success. Whether the
work succeeds in the real world is a human judgment that happens after
Trail's job is complete.

---

## Structure and files

**Why does everything live under `trail/`?**
To keep Trail isolated from your project source. The `trail/` folder can be
added to `.gitignore` easily, excluded from builds, or ignored by tools that
don't need it. One folder, clean boundary.

**Can inputs live outside the Trail folder structure?**
Yes — on a file share, network drive, cloud storage, or anywhere else. What
matters is declaration, not location. If an input lives outside `trail/`,
it must be declared in `intent.md` under Dependencies & Files with enough
information for the executor to locate it. Undeclared inputs do not exist.

**Why timestamp run folders instead of sequential (run-01, run-02)?**
Timestamps are unambiguous without context. `run-01` inside `intent-0003`
tells you nothing about when it happened or how it relates to runs in other
intents. `run-2026-03-01-09-00-00` is self-describing, collision-free, and
sortable without a manifest.

**What is `start-dev-prompt.md` for?**
It is the exact prompt the human copies into the Developer agent to begin
execution. It contains no policy or scope — just the invocation. Having it
as a file means the human entry point is part of the record, not something
reconstructed from memory each time.