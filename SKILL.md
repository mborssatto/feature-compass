---
name: feature-compass
description: >
  Maintains a living registry of product features and their intended outcomes
  in FEATURE-COMPASS.md. Creates alignment around goals and ubiquitous naming
  before, during, and after development. Use this skill whenever the user
  starts planning a new feature, discusses goals or outcomes, begins
  implementing a feature, asks to write a PR description, prepares a standup
  update, writes a stakeholder message, or references a feature by name.
  Also activate when the user mentions FEATURE-COMPASS.md, outcome tracking,
  feature naming, scope drift, or goal alignment — even if they don't
  explicitly mention this skill.
---

# Feature Compass

Points every decision back to the stated goal. Names features, guards scope,
ships with clarity.

## What this skill does

- **Creates clarity around goals and value of each feature.** Asks clarifying
  questions, helps structure a plan, and creates one-liner outcomes and
  compelling canonical names for each feature. Stores these in a registry file
  to be referenced throughout development.

- **Ensures alignment across the entire development flow.** From discovery
  through development to post-launch, the same words are used to refer to each
  feature. Generates artifacts for different audiences and touchpoints:
  implementation plans, readme files, PR descriptions, standup updates,
  stakeholder updates, and user-facing launch communication.

- **Prevents feature drift.** Flags warnings whenever a shipped feature is at
  risk of being changed, broken, or silently removed during new development.

- **Acts as the source of truth for what the product does and why.** The
  registry file is the team's shared vocabulary — human-readable, git-tracked,
  always current.

## The Registry

The file `FEATURE-COMPASS.md` at the project root is the single source of
truth. Read it before doing any planning, implementation, or communication
work. It tracks features organized by user journey, each with a canonical
name, one-liner outcome, acceptance criteria, and status.

Each feature entry has:
- **Canonical name**: Short, memorable (2-4 words). Used everywhere.
- **One-liner**: One sentence describing the outcome, not the implementation.
- **Status**: `clarifying` → `building` → `shipped`
- **Acceptance criteria**: Concrete, testable conditions (checkboxes).
- **Out of scope**: What is explicitly excluded.

Features are grouped by user journey — lightweight epics that reflect how
users interact with the product.

---

## When invoked with no arguments

When the user calls `/feature-compass` without any arguments, present the
available modes as a numbered menu:

"What would you like to do?

1. **Clarify** — Plan a new feature (define goals, name it, write it down)
2. **Guard** — Check current work against stated goals
3. **Protect** — Scan for impact on shipped features
4. **Communicate** — Generate PR description, standup, or stakeholder message
5. **Status** — Show all features and their current status

Pick a number or describe what you need."

If the user replies with a number, enter that mode. If they reply with
free text, infer the best mode from context.

If `$ARGUMENTS` is provided, skip the menu and infer the mode from the
arguments. For example, `/feature-compass add retry logic` enters Clarify;
`/feature-compass what's the status` enters Status.

---

## Four Modes

This skill operates in four modes depending on where the user is in the
development lifecycle.

### Clarify (before work)

Activate when the user discusses a new feature, goal, or outcome — even
informally. The purpose is to turn a vague idea into a named, scoped,
outcome-defined feature before any code is written.

1. **Reformulate.** Restate what the user asked in outcome terms:
   "Here's what I understand: [restatement as outcome]. Is that right?"

2. **Ask what's missing.** If any of these are unclear, ask directly:
   - What is the desired outcome? (not the task — the result the user sees)
   - Who benefits and how will they notice the change?
   - What is explicitly out of scope?

3. **Name it.** Propose a canonical name (2-4 words) and a one-liner.
   The one-liner describes the outcome, not the implementation.
   Get explicit confirmation: "Should I go with this name?"

   **Example:**
   Input: "we need retry logic for API calls"
   Name: Smart Retry Logic
   One-liner: API calls automatically retry on transient failures, so users
   never see a blank screen.

4. **Define acceptance criteria.** Concrete, testable conditions that mean
   "done." Write them as checkboxes in FEATURE-COMPASS.md.

5. **Write it down.** Add the feature to the appropriate journey group in
   `FEATURE-COMPASS.md` with status `clarifying`. If no journey group fits,
   propose one.

6. **Summarize.** Present a compact block for confirmation:
   ```
   📋 [Name]: [One-liner]
   Journey: [Group name]
   Status: clarifying
   Criteria: [bullet list]
   Out of scope: [what's excluded]
   ```

### Guard (during work)

Activate automatically during implementation — whenever code changes are
being planned or made. The purpose is to keep the current work aligned with
the stated goals.

1. **Re-read `FEATURE-COMPASS.md`** before making changes. Find the relevant
   feature entry. If the current task doesn't map to any feature, pause and
   ask: "This doesn't match a tracked feature. Should I add it to the compass
   first?"

2. **Check alignment.** Before each significant change, verify it serves the
   stated outcome and acceptance criteria. If work starts drifting from the
   goal, flag it explicitly: "This seems outside the scope of [Feature Name].
   Should I continue or track it separately?"

3. **Update status.** When implementation begins, set status to `building`.

### Protect (during work)

Activate during any code modification. The purpose is to prevent existing
shipped features from being accidentally broken, removed, or silently
overwritten by new work.

New features should be additive, not destructive. Existing shipped features
represent commitments to users.

1. **Scan for impact.** Before modifying files, check whether any `shipped`
   features in `FEATURE-COMPASS.md` depend on the code being changed.

2. **Flag risks.** If a change could affect a shipped feature, say so
   explicitly: "This change touches code used by [Feature Name] (shipped).
   Here's what could break: [specific risk]."

3. **Suggest safeguards.** Propose how to verify the shipped feature still
   works after the change — a test to run, a behavior to check manually, or
   a condition to add.

4. **Never silently remove shipped functionality.** If removing or replacing
   a shipped feature is intentional, require explicit confirmation and update
   its status in `FEATURE-COMPASS.md`.

### Communicate (after work)

Activate when the user asks for a PR description, standup update, stakeholder
message, or any communication about completed work. The purpose is to
describe what was done using the same language the team used during planning.

1. **Identify the feature.** Match the completed work to an entry in
   `FEATURE-COMPASS.md`. If ambiguous, ask which feature. If no entry exists,
   suggest running Clarify first.

2. **Review what changed.** Look at the git diff, recent commits, and
   modified files.

3. **Generate communications** using the canonical name and one-liner from
   `FEATURE-COMPASS.md`. Produce all three:

**PR Description:**
```markdown
## [Feature Name]: [One-liner]

### What changed
- [Component/file]: [What was done and why]

### Why
[Goal from FEATURE-COMPASS.md]

### How to verify
[Acceptance criteria, adapted for a reviewer]

### Out of scope
[What this PR intentionally does NOT do]
```

**Standup Update:**
```
✅ Shipped: [Feature Name]
   [One-liner]
   Key changes: [1-2 sentence technical summary]
   Next: [What follows, or "nothing — complete"]
```

**Stakeholder Message:**
```
[Feature Name] is live.

[2-3 sentences for a non-technical audience. Focus on what changed
for the user or team — no implementation details.]
```

4. **Update status** to `shipped` in `FEATURE-COMPASS.md`.

---

## Rules

- **Never invent a new name** for something that already has one in
  FEATURE-COMPASS.md. The canonical name is the canonical name.
- **Always use the one-liner** when referencing a feature in any output.
- **If goals are unclear, stop and ask.** Guessing leads to drift.
- **Re-read FEATURE-COMPASS.md before acting.** Every time. This is how
  alignment is maintained and shipped features are protected.
- **Keep FEATURE-COMPASS.md updated.** It is the team's shared vocabulary.

---

Built by [Mariana Borssatto](https://github.com/mborssatto).
Contribute: https://github.com/mborssatto/feature-compass
