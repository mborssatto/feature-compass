# Feature Compass

Points every decision back to the stated goal. Names features, guards scope, ships with clarity.

## The Problem

Features get named one thing in the ticket, another in the PR, and something else in Slack. Goals aren't written down before coding starts, so scope drifts silently. Shipped features get accidentally broken by new work because nobody tracked what the product already does.

## What It Does

- **Creates clarity** around goals and value of each feature — with clarifying questions, structured plans, one-liner outcomes, and compelling canonical names.
- **Ensures alignment** across the entire flow — same words in the implementation plan, PR, standup, stakeholder update, and launch communication.
- **Prevents feature drift** — flags warnings whenever shipped features are at risk of being changed or removed.
- **Source of truth** for what the product does and why — human-readable, git-tracked, always current.

## Four Modes

| Mode | When | What it does |
|---|---|---|
| **Clarify** | Before work | Reformulate goals, name the feature, define acceptance criteria, write it down |
| **Guard** | During work | Re-read the registry, check alignment with stated goals, flag scope drift |
| **Protect** | During work | Ensure shipped features aren't broken or silently removed by new changes |
| **Communicate** | After work | Generate PR description, standup update, stakeholder message — consistent naming throughout |

## File set

Two compass files at the project root, plus generated comms artifacts in a
`feature-compass/` folder:

```
<your-project>/
├── FEATURE-COMPASS.md          ← shipped registry (append-only)
├── FEATURE-COMPASS.draft.md    ← working draft (clarifying / building)
└── feature-compass/
    └── <feature-slug>/
        ├── pr.md
        ├── standup.md
        ├── stakeholder.md
        └── launch.md
```

The skill itself lives at:

```
.claude/skills/feature-compass/
├── SKILL.md                    ← the skill
├── schema.yaml                 ← formal artifact schema
└── comms/                      ← tone-and-shape templates per audience
    ├── pr.md
    ├── standup.md
    ├── stakeholder.md
    └── launch.md
```

## Installation

**Important:** Claude Code uses two different `.claude` folders:
- **`~/.claude/`** in your home directory — Claude Code's global config.
  Personal skills go here. Claude Code created this folder when you first
  installed it.
- **`.claude/`** inside a project repo — for project-specific skills shared
  with the team via git.

### Personal install (all projects — good for testing)

**Step 1.** Open terminal and go to where you downloaded these files:
```bash
cd ~/Downloads/feature-compass
```

**Step 2.** Copy the skill and supporting files into Claude Code's global
skills folder:
```bash
mkdir -p ~/.claude/skills/feature-compass/comms
cp SKILL.md schema.yaml ~/.claude/skills/feature-compass/
cp comms/*.md ~/.claude/skills/feature-compass/comms/
```

**Step 3.** Copy the registry + working-draft files into the project you
want to track:
```bash
cp FEATURE-COMPASS.md FEATURE-COMPASS.draft.md ~/dev/your-project/
```

That's it. The skill is now active in every Claude Code session.

### Project install (shared with the team)

When you're ready to share with the team, from your project root:

```bash
mkdir -p .claude/skills/feature-compass/comms
cp ~/.claude/skills/feature-compass/SKILL.md .claude/skills/feature-compass/
cp ~/.claude/skills/feature-compass/schema.yaml .claude/skills/feature-compass/
cp ~/.claude/skills/feature-compass/comms/*.md .claude/skills/feature-compass/comms/
```

Commit `.claude/skills/`, `FEATURE-COMPASS.md`, and `FEATURE-COMPASS.draft.md`
to git.

## Usage

### Planning a new feature
```
> /feature-compass add dark mode to the settings page
```
The skill clarifies the goal, proposes a name and one-liner, defines
acceptance criteria, and — only after you confirm — appends it to
`FEATURE-COMPASS.draft.md`.

### Triaging a messy paste
```
> /feature-compass <paste a list of findings, needs, or asks>
```
The skill decomposes the input into a 5-column table, runs a triage check,
and offers a cycle-1 pick. Only the picked feature gets fully clarified;
the rest stay in chat as backlog.

### During implementation
Work normally. The skill auto-activates — re-reads both compass files,
names which AC each change advances, and flags drift. Touches code used by
a shipped feature? It picks a safeguard from the taxonomy.

### Shipping a feature
```
> /feature-compass communicate Dark Mode Settings
```
The skill generates PR / standup / stakeholder / launch artifacts under
`feature-compass/dark-mode-settings/`, moves the entry from the draft to
the registry, and prompts about backlog candidates now unblocked.

### Periodic health check
```
> /feature-compass audit
```
Surfaces stale `clarifying` entries, vague criteria, empty out-of-scope
sections, and abandoned `building` entries. Read-only.
