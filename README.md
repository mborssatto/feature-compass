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

## Structure

```
FEATURE-COMPASS.md                           ← the registry (project root)
.claude/skills/feature-compass/
└── SKILL.md                                 ← the skill
```

## Installation

This skill ships three files: `SKILL.md` (the skill), `FEATURE-COMPASS.md`
(the registry), and this README.

**Important:** Claude Code uses two different `.claude` folders:
- **`~/.claude/`** in your home directory — Claude Code's global config. Personal skills go here. Claude Code created this folder when you first installed it.
- **`.claude/`** inside a project repo — for project-specific skills shared with the team via git.

### Personal install (all projects — good for testing)

**Step 1.** Open terminal and go to where you downloaded these files:
```bash
cd ~/Downloads/feature-compass
```

**Step 2.** Copy the skill into Claude Code's global skills folder (in your home directory):
```bash
mkdir -p ~/.claude/skills/feature-compass
cp SKILL.md ~/.claude/skills/feature-compass/SKILL.md
```

**Step 3.** Copy the registry into the project you want to track:
```bash
cp FEATURE-COMPASS.md ~/dev/your-project/FEATURE-COMPASS.md
```

That's it. The skill is now active in every Claude Code session.
`FEATURE-COMPASS.md` lives in your project root so it tracks that project's features.

### Project install (shared with the team)

When you're ready to share with the team, from your project root:

```bash
mkdir -p .claude/skills/feature-compass
cp ~/.claude/skills/feature-compass/SKILL.md .claude/skills/feature-compass/SKILL.md
```

Commit `.claude/skills/` and `FEATURE-COMPASS.md` to git.

## Usage

### Planning a new feature
```
> /feature-compass add dark mode to the settings page
```
Claude clarifies the goal, proposes a name and one-liner, defines acceptance criteria, and adds it to FEATURE-COMPASS.md.

### During implementation
Work normally. The skill auto-activates — it reads FEATURE-COMPASS.md, checks alignment with stated goals, and flags drift. It also scans for impact on shipped features.

### After implementation
```
> /feature-compass communicate Dark Mode Settings
```
Claude generates a PR description, standup update, and stakeholder message — all using the canonical name from planning.

---

Built by [Mariana Borssatto](https://github.com/mborssatto).
Contribute: https://github.com/mborssatto/feature-compass
