# Initiative Creator — Claude Code Project Instructions

This repo contains skills for creating, reviewing, and submitting JIRA Initiatives.

## Available Skills (Claude Code)

| Skill | Command | Description |
|---|---|---|
| `initiative.create` | `/initiative.create` | Generate initiative from problem statement |
| `initiative.review` | `/initiative.review [file or JN-key]` | Score + auto-revise draft |
| `initiative.submit` | `/initiative.submit [file]` | Submit to JIRA via MCP |
| `initiative.speedrun` | `/initiative.speedrun` | Full pipeline, minimal interaction |
| `initiative.breakdown` | `/initiative.breakdown [JN-key or file]` | Generate Epic drafts |

## Artifact Conventions

All outputs land in `artifacts/initiatives/`:
- `initiative-{slug}-{date}.md` — initiative drafts
- `epics/epic-{initiative}-{n}-{slug}.md` — epic breakdowns

Frontmatter status lifecycle: `draft` → `reviewed` → `submitted`

## Configuration

Before using `/initiative.submit`, create `guidelines/jira-config.md` from the example:
```
cp guidelines/jira-config-example.md guidelines/jira-config.md
# edit with your Cloud ID, project key, and default assignee account ID
```

`guidelines/jira-config.md` is gitignored — your account IDs stay local.

## Skills Reference

```
.claude/skills/
├── initiative-template.md          ← canonical section template
├── initiative.create/SKILL.md      ← /initiative.create
├── initiative.review/SKILL.md      ← /initiative.review
├── initiative.submit/SKILL.md      ← /initiative.submit
├── initiative.speedrun/SKILL.md    ← /initiative.speedrun
└── initiative.breakdown/SKILL.md   ← /initiative.breakdown
```

## Writing Guidelines

- `guidelines/initiative-guidelines.md` — full template, DoD rules, best practices
- `guidelines/issue-creation-guidelines.md` — Initiative/Epic/Story quality checklist
