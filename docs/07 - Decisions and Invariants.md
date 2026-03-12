# 07 - Decisions and Invariants

# Decisions & Invariants

This page lists the current **non-negotiables** and major decisions that define Trail.

## Invariants (do not violate)

1. **Artifacts are authoritative.**
    - Conversations are not the source of truth.
2. **Intent is human-owned and defines scope.**
    - Executors (AI or human) do not decide "what" is being built.
3. **Multi-intent is the current model.**
    - Products evolve through multiple immutable intents.
4. **Intents are immutable once execution begins.**
    - New work → new intent.
    - Old intents are preserved.
    - The only exception is if the initial intent cannot be used by the Manager (INTENT_UNUSABLE).
5. **Runs are execution-only.**
    - Runs answer "how," never "what."
    - Manager decides number and sequence of runs.
6. **Role separation is enforced.**
    - Architect/Product Owner defines scope. Human only.
    - Manager produces run artifacts. Human or AI.
    - Developer executes run artifacts. Human or AI.
    - Reviewer verifies outputs. Human only.
    - Architect/Product Owner and Reviewer may be the same person. No other role merging is permitted.
7. **Ambiguity must be documented, not inferred.**
    - If something is unclear, choose the simplest interpretation consistent with stated intent, document it in `results.md`, and stop if unable to proceed.
8. **Executors must not rely on context outside declared inputs.**
    - Separate chats are optional; file-bounded context is mandatory.
9. **Intent packages include three files:**
    - `intent.md` (authoritative scope document)
    - `manager-instructions.md` (how Manager operates)
    - `operating-instructions-override.md` (intent-level policy overrides)

---

## Strong recommendations (high leverage)

- When using AI: use different models or different chats for Manager and Developer (reduces same-model drift).
- Keep `trail/meta/global-operating-instructions.md` stable; prefer intent-level overrides over frequent run-specific changes.
- Run-level `operating-instructions.md` is a composite (global + intent override + run-specific); keep run-specific policy minimal.

**Predictable input locations:** Always store inputs in one of two places:

- `trail/intents/<intent>/files/` — for intent-scoped inputs (recommended default)
- `trail/meta/files/` — for shared project inputs

Executors follow this lookup order: intent files first, meta files second. No run-level `files/` folder.

**Missing input policy:**

- If a missing input is **non-critical** (placeholder icons, copy, help text): create a reasonable placeholder, store it under the appropriate `files/` folder, and record it in `results.md`.
- If a missing input is **critical** (requirements, legal text, customer data): **do not invent it.** Stop and report the missing dependency.

---

## Naming conventions

- Intent folders:
    - `intent-0001` style, or
    - `intent-YYYY-MM-DD-mm-ss` style
- Run folders:
    - `run-YYYY-MM-DD-HH-MM-SS` (required — timestamp provides ordering, human readability, and collision avoidance)

---

## Versioning

Versioning is a human responsibility. Trail does not enforce it, automate it, or have an opinion on how you do it.

If your project needs a version reference point for executors, a `version.md` file in `trail/meta/` is a reasonable convention. That is the extent of Trail's recommendation.

To be explicit:

- Trail does not ship a `version.md` template.
- Trail does not specify a version file format.
- `version.md` is a recommendation, not a requirement.

If you don't need it, don't create it. If you do, build it however makes sense for your project.