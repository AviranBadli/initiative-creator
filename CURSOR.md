# Initiative Creator — Cursor Project Instructions

This repo contains Cursor skills for creating, reviewing, and submitting JIRA Initiatives.

## Available Commands

| Command | Description |
|---|---|
| `/initiative.create` | Generate a new initiative from a problem statement |
| `/initiative.review` | Score and auto-revise a draft (up to 2 cycles) |
| `/initiative.submit` | Submit to JIRA via Atlassian MCP |
| `/initiative.speedrun` | Full pipeline with minimal interaction |
| `/initiative.breakdown` | Generate Epic drafts from an approved initiative |

## How Skills Work

Each command loads its corresponding skill file from `.cursor/skills/initiative-creator/`. The skills reference shared guidelines from the `guidelines/` directory and save all outputs to `artifacts/initiatives/`.

## Configuration

Before using `/initiative.submit`, ensure:
1. Atlassian MCP is configured in Cursor settings
2. `guidelines/jira-config.md` exists with your team's Cloud ID, project key, and default assignee

## Artifact Lifecycle

```
artifacts/initiatives/initiative-{slug}-{date}.md
  status: draft      ← created by /initiative.create
  status: reviewed   ← updated by /initiative.review
  status: submitted  ← updated by /initiative.submit (adds jira_key)

artifacts/initiatives/epics/
  epic-{initiative}-{n}-{slug}.md  ← created by /initiative.breakdown
```

## Guidelines

- `guidelines/initiative-guidelines.md` — full template, DoD rules, writing best practices
- `guidelines/issue-creation-guidelines.md` — checklist for Initiative, Epic, and Story quality
- `.cursor/skills/initiative-creator/initiative-template.md` — canonical section template
