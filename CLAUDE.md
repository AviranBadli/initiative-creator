# Initiative Creator — Claude Code Project Instructions

This repo contains skills for creating, reviewing, and submitting JIRA Initiatives.

Skill logic lives in the **shared `skills/` directory**. The `.claude/skills/*/SKILL.md` files are thin wrappers with Claude Code frontmatter that load from `skills/`.

## Available Skills

| Command | Wrapper | Shared Skill |
|---|---|---|
| `/initiative.create` | `.claude/skills/initiative.create/SKILL.md` | `skills/initiative.create/SKILL.md` |
| `/initiative.review` | `.claude/skills/initiative.review/SKILL.md` | `skills/initiative.review/SKILL.md` |
| `/initiative.submit` | `.claude/skills/initiative.submit/SKILL.md` | `skills/initiative.submit/SKILL.md` |
| `/initiative.speedrun` | `.claude/skills/initiative.speedrun/SKILL.md` | `skills/initiative.speedrun/SKILL.md` |
| `/initiative.breakdown` | `.claude/skills/initiative.breakdown/SKILL.md` | `skills/initiative.breakdown/SKILL.md` |

## Configuration

Before using `/initiative.submit`, create `guidelines/jira-config.md` from the example:
```
cp guidelines/jira-config-example.md guidelines/jira-config.md
# edit with your Cloud ID, project key, and default assignee account ID
```

`guidelines/jira-config.md` is gitignored — your account IDs stay local.

## To Edit a Skill

Edit the file in `skills/` — both Cursor and Claude Code will automatically get the update. Never edit the wrapper files in `.claude/skills/` for logic changes.
