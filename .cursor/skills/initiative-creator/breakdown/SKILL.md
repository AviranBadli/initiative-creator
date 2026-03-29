# Initiative Breakdown Skill

Take an approved initiative and generate a structured set of Epic drafts that collectively cover the initiative scope. Save each Epic as an artifact file.

---

## Input

`$ARGUMENTS` — One of:
- A JIRA initiative key (e.g., `JN-3097`) — fetches the initiative from JIRA
- A file path to a local initiative artifact (e.g., `artifacts/initiatives/initiative-unified-logging-2026-03-29.md`)
- Nothing — auto-detect the most recently submitted initiative in `artifacts/initiatives/`

---

## Step 1 — Load the Initiative

**If a JIRA key:** Fetch via Atlassian MCP with `expand: "changelog,comments"`. Extract the full Description and Summary.

**If a file path:** Read the file directly.

**If no argument:** Find the most recently modified `.md` file in `artifacts/initiatives/` with `status: submitted` or `status: reviewed`.

Also read silently:
- `guidelines/issue-creation-guidelines.md` — Epic structure, checklist, examples

Call `mcp_time_get_current_time` with timezone `Asia/Jerusalem` for timestamps.

---

## Step 2 — Analyze Initiative Scope

Read the initiative thoroughly — multiple times if needed — focusing on:

1. **Goals** — each goal may map to one or more Epics
2. **Scope** — each scope item should be covered by at least one Epic
3. **Phases** — if phases exist, Epics within each phase should be distinct
4. **DoD** — each DoD criterion should be traceable to at least one Epic

Identify 3-7 logical work areas that together cover 100% of the initiative scope. These become the Epics.

---

## Step 3 — Ask for Scope Confirmation (Optional)

If the initiative is complex or scope areas are ambiguous, present your proposed Epic groupings for quick user approval before drafting:

```
📊 Proposed Epic Structure
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Initiative: "{title}"

Proposed Epics (covering full scope):
1. {Epic 1 title} — {1-sentence description}
2. {Epic 2 title} — {1-sentence description}
3. {Epic 3 title} — {1-sentence description}
...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Does this grouping look right? (yes / adjust Epic N / add Epic for X)
```

If the user says yes or the breakdown is straightforward → proceed directly to Step 4.

---

## Step 4 — Draft Each Epic

For each Epic, produce a full structured description following the Epic template from `jira-issue-creation-guidelines.md`:

### Epic Template

```markdown
---
status: draft
parent_initiative: JN-XXXX
epic_number: N
created: YYYY-MM-DD
---

# [Action Verb] [Feature/Capability] [Outcome]

[1-2 sentence summary of what this epic delivers.]

## Context

[1-2 paragraphs explaining:]
- How this epic contributes to the parent initiative
- What user need or technical problem it addresses
- Key dependencies or constraints

## Goals

* [Goal 1 — specific to this epic]
* [Goal 2]
* [Goal 3]

## Scope

* [Deliverable 1]
* [Deliverable 2]
* [Deliverable 3]

### Out of Scope

* [What is NOT included in this epic]
* [Deferred to another epic or phase]

## Definition of Done

* [Criterion 1 — verifiable]
* [Criterion 2 — verifiable]
* [Criterion 3 — verifiable]

## Questions & Answers

### Open Questions
(None yet)

### Answered Questions
(None yet)

## Notes

* Parent Initiative: {JN-XXXX or file path}
* Related Epics: [list sibling epics by number]
* Estimated size: 1-3 sprints
```

### Epic Title Rules (from jira-issue-creation-guidelines.md)
- Must start with an action verb
- 60-80 characters max
- Format: `[Action Verb] + [Feature/Capability] + [Outcome]`
- Examples: "Build cost tracking dashboard for benchmark runs", "Implement real-time benchmark monitoring UI"

### Epic Content Rules
- Context explains how this epic fits the larger initiative (reference the parent)
- Goals are specific to this epic's scope only (not the full initiative)
- DoD is achievable within 1-3 sprints
- Each epic can be broken into 3-10 Stories later
- Out of scope explicitly calls out what goes to sibling epics

---

## Step 5 — Save Epic Artifacts

Save each Epic to:
```
artifacts/initiatives/epics/epic-{initiative-slug}-{n}-{epic-slug}.md
```

Where:
- `{initiative-slug}` = slug from the parent initiative file name or JN key
- `{n}` = Epic number (01, 02, 03...)
- `{epic-slug}` = 3-4 word kebab-case slug of the Epic title

Example:
```
artifacts/initiatives/epics/
├── epic-unified-logging-01-log-aggregation-pipeline.md
├── epic-unified-logging-02-structured-error-taxonomy.md
├── epic-unified-logging-03-opensearch-ui-integration.md
└── epic-unified-logging-04-customer-facing-api.md
```

---

## Step 6 — Coverage Check

After drafting all Epics, verify:

- Every initiative Scope item is covered by at least one Epic
- Every initiative DoD criterion is traceable to at least one Epic's DoD
- No Epic is so large it would take more than 3 sprints (if so, split it)
- No two Epics have overlapping scope (if so, clarify boundaries)

If coverage gaps exist → add an additional Epic or expand an existing one.

---

## Step 7 — Output Summary

Present the breakdown to the user:

```
✅ Epic Breakdown Complete

Initiative: "{title}" ({JN-key or file})
Generated:  {N} Epics covering full initiative scope

Epics created:
1. [{Epic 1 title}](path) — {1-sentence description}
2. [{Epic 2 title}](path) — {1-sentence description}
3. [{Epic 3 title}](path) — {1-sentence description}
...

Coverage check: ✅ All initiative scope items covered

Files saved to: artifacts/initiatives/epics/

Next steps:
- Review individual Epic files and refine
- Create Epics in JIRA under initiative {JN-key}
- Break each Epic into Stories using the Story template in jira-issue-creation-guidelines.md
```
