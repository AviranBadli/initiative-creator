# Initiative Creator — System Entry Point

This is the Cursor-facing entry point for the initiative-creator skill system.

All actual skill logic lives in the shared `skills/` directory at the repo root, used by both Cursor and Claude Code.

## Available Commands

| Command | Shared Skill | What it does |
|---|---|---|
| `/initiative.create` | `skills/initiative.create/SKILL.md` | Generate initiative from problem statement |
| `/initiative.review` | `skills/initiative.review/SKILL.md` | Score + auto-revise draft |
| `/initiative.submit` | `skills/initiative.submit/SKILL.md` | Submit to JIRA via MCP |
| `/initiative.speedrun` | `skills/initiative.speedrun/SKILL.md` | Full pipeline, minimal interaction |
| `/initiative.breakdown` | `skills/initiative.breakdown/SKILL.md` | Generate Epic drafts |

## Shared Resources

- `skills/initiative-template.md` — canonical section template
- `guidelines/initiative-guidelines.md` — writing rules and best practices
- `guidelines/issue-creation-guidelines.md` — checklist for all issue types
- `guidelines/jira-config.md` — your JIRA configuration (gitignored)

## Artifact Output

All drafts saved to `artifacts/initiatives/` with YAML frontmatter tracking status lifecycle:
`draft` → `reviewed` → `submitted`
