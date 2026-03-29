# Initiative Creator — Cursor Project Instructions

This repo contains skills for creating, reviewing, and submitting JIRA Initiatives.

Skill logic lives in the **shared `skills/` directory**. The `.cursor/commands/*.md` files are thin wrappers that load from `skills/`.

## Available Commands

| Command | Wrapper | Shared Skill |
|---|---|---|
| `/initiative.create` | `.cursor/commands/initiative.create.md` | `skills/initiative.create/SKILL.md` |
| `/initiative.review` | `.cursor/commands/initiative.review.md` | `skills/initiative.review/SKILL.md` |
| `/initiative.submit` | `.cursor/commands/initiative.submit.md` | `skills/initiative.submit/SKILL.md` |
| `/initiative.speedrun` | `.cursor/commands/initiative.speedrun.md` | `skills/initiative.speedrun/SKILL.md` |
| `/initiative.breakdown` | `.cursor/commands/initiative.breakdown.md` | `skills/initiative.breakdown/SKILL.md` |

## Artifact Lifecycle

```
artifacts/initiatives/initiative-{slug}-{date}.md
  status: draft      ← created by /initiative.create
  status: reviewed   ← updated by /initiative.review
  status: submitted  ← updated by /initiative.submit (adds jira_key)

artifacts/initiatives/epics/
  epic-{initiative}-{n}-{slug}.md  ← created by /initiative.breakdown
```

## To Edit a Skill

Edit the file in `skills/` — both Cursor and Claude Code will automatically get the update. Never edit the wrapper files in `.cursor/commands/` for logic changes.

## Guidelines

- `guidelines/initiative-guidelines.md` — full template, DoD rules, writing best practices
- `guidelines/issue-creation-guidelines.md` — checklist for Initiative, Epic, and Story quality
- `skills/initiative-template.md` — canonical section template
