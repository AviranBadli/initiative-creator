# Initiative Creator

Skills for creating, reviewing, and submitting JIRA Initiatives — from a problem statement to a live ticket, with Epic breakdown support.

Works with both **Cursor** (`.cursor/`) and **Claude Code** (`.claude/`).

Inspired by [jwforres/rfe-creator](https://github.com/jwforres/rfe-creator), which established the pipeline pattern and multi-step review concept.

## Quick Start

```
# Core Pipeline
/initiative.create      # Write a new initiative from a problem statement
/initiative.review      # Review, improve, and auto-revise an initiative
/initiative.submit      # Submit new initiative to JIRA
/initiative.speedrun    # Full pipeline end-to-end with minimal interaction
/initiative.breakdown   # Break an approved initiative into Epic drafts

# Work on existing initiatives
/initiative.review JN-3097       # Fetch, review, and auto-revise
/initiative.update JN-3097       # Fetch, edit, and re-submit an existing initiative
/initiative.status JN-3097       # Live status: epics done/in-progress/blocked
/initiative.status JN-3097 JN-3112  # Status for multiple initiatives at once

# Quality and planning
/initiative.feasibility JN-3097  # 3-angle adversarial review (technical, scope, timeline)
/initiative.split JN-3097        # Split an oversized initiative into right-sized pieces

# Epics
/initiative.breakdown JN-3097    # Generate all epics from an initiative at once
/epic.create JN-3097             # Create a single specific epic under an initiative
```

> Same commands work in both Cursor and Claude Code.

## Pipeline

### New Initiatives

```
/initiative.create → /initiative.review → /initiative.submit
```

`/initiative.review` auto-revises issues it finds (up to 2 cycles). You can edit artifacts manually between steps.

`/initiative.speedrun` runs the full pipeline with reasonable defaults and minimal interaction.

### Existing JIRA Initiatives

```
/initiative.review JN-3097 → /initiative.submit
```

Or in one step: `/initiative.speedrun JN-3097`

### Epic Breakdown (after initiative is approved)

```
/initiative.breakdown JN-3097
```

Generates 3-7 Epic drafts that collectively cover the full initiative scope.

## Skill Reference

| Skill | Command | Description |
|---|---|---|
| Create | `/initiative.create` | Clarifying questions + RFE/initiative discovery → full structured draft |
| Review | `/initiative.review` | 100-point checklist score + up to 2 auto-revise cycles |
| Submit | `/initiative.submit` | JIRA MCP submission with full payload approval gate |
| Speedrun | `/initiative.speedrun` | create → review → submit with only 2 user pauses |
| Breakdown | `/initiative.breakdown` | Generate all Epics from an initiative (full scope coverage) |
| Update | `/initiative.update` | Fetch existing initiative, apply edits, re-submit to same ticket |
| Status | `/initiative.status` | Live JIRA snapshot: epics done/blocked + velocity estimate |
| Feasibility | `/initiative.feasibility` | 3-angle adversarial review: technical, scope, timeline |
| Split | `/initiative.split` | Split an oversized initiative into right-sized pieces |
| Epic Create | `/epic.create` | Create a single Epic under an initiative with overlap detection |

## Editing Between Steps

All artifacts are written to `artifacts/initiatives/`. You can edit any file between steps:

- Edit a draft at `artifacts/initiatives/initiative-{slug}-{date}.md`, then re-run `/initiative.review`
- Re-run `/initiative.create` to start over from scratch

Epic drafts are saved to `artifacts/initiatives/epics/`.

## What Makes a Good Initiative

Every initiative produced by this pipeline enforces:

- **Title**: Action verb + what + purpose, 60-80 characters
- **Context**: 2-4 paragraphs explaining WHY this exists (not just WHAT)
- **Goals**: 3-6 specific, measurable outcomes
- **Scope**: Explicit in-scope and out-of-scope boundaries
- **Expected Impact**: Quantifiable outcomes where possible
- **Definition of Done**: Verifiable, non-ambiguous completion criteria
- **Q&A Section**: Open questions tracked with owners and dates

## Configuration

Copy `guidelines/jira-config-example.md` and fill in your team's values:

| Field | Description |
|---|---|
| Cloud ID | Your Atlassian cloud identifier |
| Project Key | Your JIRA project (e.g., `JN`, `RHAIRFE`) |
| Default Assignee | JIRA account ID of the default assignee |
| Default Priority | Medium / High / Critical |

The skills read configuration from `guidelines/jira-config.md` if it exists, otherwise they prompt you for the values.

## JIRA MCP

If the Atlassian MCP server is configured in Cursor, `/initiative.submit` creates tickets directly. Without it, the skill produces a formatted manual submission guide.

## Artifact Schema

```yaml
---
status: draft | reviewed | submitted
jira_key: ""          # filled in after /initiative.submit
created: YYYY-MM-DD
title: ""
assignee: ""          # JIRA account ID
priority: Medium
---
```

## File Structure

```
skills/                               ← SHARED source of truth (edit here, not in wrappers)
├── initiative-template.md            ← canonical section template
├── initiative.create/SKILL.md        ← actual create logic
├── initiative.review/SKILL.md        ← actual review logic
├── initiative.submit/SKILL.md        ← actual submit logic
├── initiative.speedrun/SKILL.md      ← actual speedrun logic
└── initiative.breakdown/SKILL.md     ← actual breakdown logic

.claude/                              ← Claude Code thin wrappers
├── settings.json
└── skills/
    ├── initiative.create/SKILL.md    ← frontmatter + "load skills/initiative.create/SKILL.md"
    ├── initiative.review/SKILL.md
    ├── initiative.submit/SKILL.md
    ├── initiative.speedrun/SKILL.md
    └── initiative.breakdown/SKILL.md

.cursor/                              ← Cursor thin wrappers
├── skills/initiative-creator/
│   └── SKILL.md                      ← system overview, points to skills/
└── commands/
    ├── initiative.create.md           ← "load skills/initiative.create/SKILL.md"
    ├── initiative.review.md
    ├── initiative.submit.md
    ├── initiative.speedrun.md
    └── initiative.breakdown.md

artifacts/initiatives/                ← draft outputs (gitignored)
└── epics/                            ← epic breakdown outputs

guidelines/
├── initiative-guidelines.md          ← writing rules and best practices
├── issue-creation-guidelines.md      ← checklist for all issue types
├── jira-config-example.md            ← config template
└── jira-config.md                    ← your team's config (gitignored)

CLAUDE.md                             ← Claude Code project instructions
CURSOR.md                             ← Cursor project instructions
```

> **To update a skill:** edit the file in `skills/` only. Both Cursor and Claude Code pick it up automatically — never edit the wrappers.
