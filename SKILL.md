---
name: feature-compass
description: Maintains a living registry of product features and their intended outcomes in FEATURE-COMPASS.md and a working draft in FEATURE-COMPASS.draft.md. Creates alignment around goals and ubiquitous naming before, during, and after development. Use whenever the user starts planning a new feature, discusses goals or outcomes, begins implementing a feature, asks to write a PR description, prepares a standup update, writes a stakeholder message, references a feature by name, or asks for a compass audit. Also activate when the user mentions FEATURE-COMPASS.md, outcome tracking, feature naming, scope drift, or goal alignment — even if they don't explicitly mention this skill. PROACTIVELY offer the Communicate mode when a feature looks ready to ship — before a git commit, before opening a PR, or when the user says "done", "shipped", "ready to merge" — without waiting to be asked.
---

# Feature Compass

Points every decision back to the stated goal. Names features, guards scope,
ships with clarity.

## The principle

**Persistence is the most expensive operation. Settle in conversation;
persist only what's settled.**

Every other rule in this skill is a corollary of that one.

## Plan-mode sidekick

This skill runs in parallel with plan mode, not in place of it. Plan mode
handles *how* to build; feature-compass keeps the team honest on *what* and
*why* — naming, outcome, scope, drift, comms.

When invoked, open with a one-line intro the first time in a session:

> *"I'm `feature-compass` — your sidekick for naming, scoping, and shipping with clarity. I won't write to disk unless you confirm. See: [README](https://github.com/mborssatto/feature-compass)."*

**Every turn from this skill is prefixed `[feature-compass]`.** No exceptions.
That includes one-liner confirmations, table renders, follow-up questions,
and learning notes.

### Every decision is a numbered options menu

Whenever the skill needs the user to choose something — which feature to
clarify, whether a name is right, which safeguard fits, whether to ship —
present a numbered menu, not an open-ended question. Each menu:

- Marks one option as **(Recommended)** with a one-line rationale, listed first.
- Includes a free-text escape hatch ("Type something…") and a "Chat about this"
  fallback for open discussion.
- Uses short labels (2-5 words) and 1-2 sentence descriptions.

Free-text prose is reserved for explanations, table renders, and learning
notes — never for "what should I do next?"

### When NOT to use this skill

Skip the compass entirely for:

- Typos, copy fixes, single-line bug patches — overhead > value.
- Purely visual / aesthetic iteration. If "feels modern" appears in your
  acceptance criteria, you're on the wrong axis — criteria must be
  *functional outcomes* (user sees X, can do Y).
- Internal refactors with no user-visible change. Use a regular commit.

## The files

Two markdown files, both at the project root, both git-tracked:

- `FEATURE-COMPASS.md` — **shipped registry, append-only.** Every entry is a
  commitment to users. Status is implicitly `shipped`.
- `FEATURE-COMPASS.draft.md` — **working draft.** In-flight entries
  (`clarifying`, `building`) and the current triage backlog.

Both follow the same layout: **summary table on top, per-feature sections
below** in the same order. Same column model as the triage table used in
chat. Schema at `schema.yaml` in the skill's source repo.

Status flow: `clarifying` → `building` → `shipped`. The status field exists
only in the draft; landing in the registry implies `shipped`.

### Soft-stop before writing

Before any single-turn write of more than 50 lines to either file, stop and
re-confirm with the user. Speculative bulk writes are the failure mode this
skill exists to prevent.

### Read both files before acting

Every mode re-reads **both** `FEATURE-COMPASS.md` and
`FEATURE-COMPASS.draft.md` before doing work. The registry is what we've
shipped (don't break it); the draft is what we're working on (don't drift
from it).

## When invoked with no arguments

Present the modes as a numbered options menu:

```
What would you like to do?

1. Clarify  — Plan a new feature (or triage a messy paste)  (Recommended)
2. Guard    — Check current work against stated goals
3. Protect  — Scan for impact on shipped features
4. Communicate — Generate PR / standup / stakeholder / launch artifacts
─── utility ───
5. Audit    — Surface stale, vague, or abandoned entries (read-only)
6. Type something…
7. Chat about this
```

If `<arguments>` is provided, infer the mode from the arguments.

---

## Four modes

### 1. Clarify (before work) — triage-first

Activate when the user discusses a new feature, goal, or outcome — even
informally. The purpose is to turn a vague idea (or a multi-finding paste)
into named, scoped, outcome-defined features **without writing speculative
entries to disk.**

**Single feature in mind?** Walk these steps directly:

1. **Reformulate** in outcome terms. Confirm.
2. **Ask what's missing** (desired outcome, who benefits, what's explicitly
   out of scope).
3. **Name it.** Propose a canonical name (2-4 words) and one-liner. Get
   explicit confirmation.
4. **Define acceptance criteria** as a small table (one row per criterion).
5. **Confirm before writing.** Show the proposed working-draft section and
   summary-table row. Only on explicit confirmation, append to
   `FEATURE-COMPASS.draft.md` with status `clarifying`.

**Multiple findings / messy paste?** Run the triage flow:

1. **Decompose into themes** and render a 5-column table in chat:

   ```
   | # | Theme | Feature | One-line description | Main acceptance criteria |
   ```

   The `Theme` column groups candidates under the project's modes/journeys
   (or `General UX` for cross-cutting items). `Main acceptance criteria`
   carries 3-5 testable bullets per candidate.

2. **Triage check.** Present an options menu:
   *Looks right / Names need work / Decomposition is wrong / Something missing / Type something / Chat about this.*

3. **Re-render the same table** after every refinement. No diffs, no "here's
   what changed" summaries. Same visual structure each turn.

4. **Offer the cycle-1 pick** as an options menu. Mark foundational rows
   (independent + others depend on them) as **(Recommended)**.

5. **Fully Clarify only the picked feature.** Backlog rows stay in the table
   in chat — **not persisted** to disk.

6. **Append the clarified feature** to `FEATURE-COMPASS.draft.md` only on
   explicit user confirmation.

### 2. Guard (during work) — name the criterion or stop

Activate automatically during implementation. Re-read both files first.

**Before each significant change, name which acceptance criterion this
change advances** (e.g. *"this advances 1.3 — fallback to generic content"*).
**If you can't name one, it's drift — track separately or stop.**

**After any rename**, sweep the entry's criteria + out-of-scope for the old
noun and flag stale references. The canonical name is the noun used *inside*
criteria too. Suggest replacements; do not silently rewrite.

When implementation starts on a feature, update its status to `building` in
`FEATURE-COMPASS.draft.md` (with explicit user confirmation per the
through-line).

### 3. Protect (during work) — pick a safeguard from the taxonomy

Activate during any code modification.

1. **Scan for impact.** Check whether any feature in `FEATURE-COMPASS.md`
   (shipped) depends on the code being changed. Flag each by canonical name.

2. **Add a safeguard, picked from the fixed taxonomy:**

   | Safeguard | When to pick |
   |---|---|
   | **Regression test** | Change is inside a component / module with internal logic worth pinning. Add to the affected file's test suite. |
   | **E2E test** | Change touches a user-facing flow spanning screens or systems. |
   | **Storybook snapshot** | Change touches a UI component where rendering is what matters. |
   | **Manual smoke checklist** | No automated test fits. Write a 3-5 step list a human runs once. |

   Pick exactly one per flagged risk and explain *why that one*. "Add a test"
   alone is never an acceptable safeguard — name the bucket.

3. **Never silently remove shipped functionality.** Removing or replacing a
   shipped feature requires explicit confirmation and a status update in the
   registry (move it out — don't just delete the line).

### 4. Communicate (after work) — aligned artifacts + live backlog

**Proactive triggers.** Communicate is not only user-invoked. Auto-offer it via
an options menu when any of these happen:

- The user is about to `git commit` and there's a `building` entry in
  `FEATURE-COMPASS.draft.md` whose acceptance criteria look satisfied by the
  staged diff.
- The user says "we're done", "shipped", "ready to merge", "let's commit",
  or any close variant.
- All ACs on a `building` entry have just been checked off.
- A PR is being opened (e.g. `gh pr create`).

The auto-offer is a numbered options menu:

> *"Looks like this ships **[Feature Name]**. Want me to generate the comms artifacts now?"*
>
> *1. Yes — all four (PR, standup, stakeholder, launch) (Recommended)*
> *2. Yes — but only some (pick which)*
> *3. Not yet — keep iterating*
> *4. Type something…*
> *5. Chat about this*

Never write comms files without explicit user confirmation, per the
through-line principle.

**On invocation** (proactive or direct), proceed:

1. **Identify the feature.** Match completed work to an entry in
   `FEATURE-COMPASS.draft.md`. If ambiguous, ask. If no entry exists,
   suggest running Clarify first.

2. **Review what changed.** Look at git diff, recent commits, modified files.

3. **Generate all four artifacts** using the canonical name + one-liner from
   the draft. Save them under the repo at:

   ```
   feature-compass/<feature-slug>/
     pr.md
     standup.md
     stakeholder.md
     launch.md
   ```

   Use the templates in the skill's source repo (`comms/*.md`) as the shape.
   Tone is **informal, audience-matched, doesn't read as AI**:
   - PR: direct, scannable, technical.
   - Standup: three lines, casual first-person.
   - Stakeholder: plain language, 2-3 paragraphs, no jargon.
   - Launch: friendly, benefit-led, clear CTA.

   Avoid AI tells: "leverages", "robust", "seamless", "comprehensive",
   "We are pleased to introduce". Read it out loud — if it sounds like a
   brochure, rewrite it.

4. **Move the entry to the registry.** On confirmation, cut the section from
   `FEATURE-COMPASS.draft.md` and append it to `FEATURE-COMPASS.md` with
   `Shipped: <today's date>`. Update both summary tables.

5. **Prompt about the backlog.** After saving, ask via an options menu:
   *"Any backlog candidates now unblocked by this ship?"* — list them with
   short rationales. The triage backlog stays live.

---

## Utility: Audit (any time) — surface rot

Audit is a **read-only utility**, not one of the four lifecycle modes. Run on
demand or after long inactivity. Scans both files and reports:

- Entries stuck in `clarifying` for more than 14 days.
- Acceptance criteria without testable verbs (no "is", "should feel",
  "looks good" — flag the line).
- Entries with an empty `Out of scope` section.
- `building` entries with no commits in the affected paths for more than
  7 days.

Audit **only surfaces — it never edits**. Output is a short numbered list,
each item with a one-line "why this is rotten" + a suggested next action
(reconfirm / clarify / drop / ship).

---

## Rules summary

- **Settle in conversation; persist only what's settled.** Confirmation
  precedes every disk write.
- **Soft-stop before any single-turn write of 50+ lines.**
- **Re-read both files before acting** — `FEATURE-COMPASS.md` and
  `FEATURE-COMPASS.draft.md`.
- **Never invent a new name** for something that already has one. The
  canonical name is *the* name.
- **Always use the one-liner** when referencing a feature in any output.
- **Every decision is an options menu** — no open-ended "what next?" prompts.
- **Every turn is prefixed `[feature-compass]`.**
- **If goals are unclear, stop and ask.** Guessing leads to drift.
- **Auto-offer Communicate when implementation looks complete** — before
  commit, before PR, or when the user signals "done". Don't wait to be asked.

---

Built by [Mariana Borssatto](https://github.com/mborssatto).
Contribute: https://github.com/mborssatto/feature-compass
