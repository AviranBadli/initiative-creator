# Epic Create Skill

Generate a single well-structured JIRA Epic under a specified initiative. More focused than `/initiative.breakdown` (which creates all epics at once) — use this when you need to add one specific epic.

---

## Input

`$ARGUMENTS` — One of:
- `JN-XXXX "Epic description"` — initiative key + description of the epic to create
- Just a description — the skill will ask which initiative it belongs to
- Nothing — the skill will ask both

---

## Step 1 — Load Context

Read silently:
- `guidelines/initiative-guidelines.md`
- `guidelines/issue-creation-guidelines.md` (focus on the Epic Guidelines section)
- `guidelines/jira-config.md`

Call `mcp_time_get_current_time` with timezone `Asia/Jerusalem`.

---

## Step 2 — Load Parent Initiative

If an initiative key was provided, fetch it from JIRA:
```
jira_get_issue(issue_key: "JN-XXXX", expand: "description,comments")
```

Also fetch existing epics under this initiative to understand what's already covered:
```
jira_search_issues(jql: 'project = JN AND issuetype = Epic AND "Initiative Link" = JN-XXXX')
```

If no initiative key was provided, ask:
```
Which initiative should this epic belong to?
(Provide a JN-XXXX key, or describe the initiative and I'll search for it)
```

---

## Step 3 — Clarifying Questions

Ask in a single message — skip any already answered in `$ARGUMENTS`:

1. **What does this epic deliver?** What specific feature, capability, or work area does it cover? (1-2 sentences)
2. **How does it fit the initiative?** Which part of the parent initiative's scope does this epic address?
3. **What's the Definition of Done?** How will we know this epic is complete? (3 verifiable criteria minimum)
4. **What's explicitly out of scope?** What does this epic NOT cover that someone might assume it does?
5. **Size check:** Should this be completable in 1-3 sprints? If larger, should it be split?
6. **Assignee:** Who owns this epic? (defaults to initiative assignee if not specified)

---

## Step 4 — Check for Overlap

Before drafting, verify the new epic doesn't duplicate an existing one:

If existing epics were found in Step 2, check:
- Does any existing epic already cover this area?
- If yes, present the overlap to the user and ask: "This overlaps with `JN-XXX "{existing epic title}"`. Should I extend that epic instead, or create a new one with clear boundaries?"

---

## Step 5 — Draft the Epic

Produce a complete epic using the Epic structure from `guidelines/issue-creation-guidelines.md`:

### Title
- Format: `[Action Verb] [Feature/Capability] [Outcome]`
- 60-80 characters max
- Must start with an action verb

### Full Description Template

```markdown
# [Epic Title]

[1-2 sentence summary of what this epic delivers.]

## Context

[1-2 paragraphs:]
- How this epic contributes to the parent initiative: {JN-XXXX}
- What user need or technical problem it addresses
- Key dependencies or constraints (reference sibling epics if needed)

## Goals

* [Goal 1 specific to this epic]
* [Goal 2]
* [Goal 3]

## Scope

* [Deliverable 1]
* [Deliverable 2]
* [Deliverable 3]

### Out of Scope

* [What is NOT included — be specific, reference sibling epics if relevant]

## Definition of Done

* [Criterion 1 — verifiable yes/no]
* [Criterion 2 — verifiable yes/no]
* [Criterion 3 — verifiable yes/no]

## Questions & Answers

### Open Questions
(None yet)

### Answered Questions
(None yet)

## Notes

* Parent Initiative: [JN-XXXX](https://{org}.atlassian.net/browse/JN-XXXX)
* Related Epics: [list siblings if known]
* Estimated size: 1-3 sprints
```

---

## Step 6 — Save Artifact

Save to:
```
artifacts/initiatives/epics/epic-{initiative-slug}-{n}-{epic-slug}.md
```

With frontmatter:
```yaml
---
status: draft
jira_key: ""
parent_initiative: JN-XXXX
epic_number: N
created: YYYY-MM-DD
title: ""
assignee: ""
priority: Medium
---
```

---

## Step 7 — Approval Preview

Show the epic title + DoD for quick user confirmation before offering to submit:

```
📋 Epic Draft Ready
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Title:  "{epic title}"
Parent: JN-XXXX "{initiative title}"
File:   artifacts/initiatives/epics/epic-{slug}.md

Definition of Done:
  • {criterion 1}
  • {criterion 2}
  • {criterion 3}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Options:
  a) Submit to JIRA now
  b) Save draft only (submit later with /initiative.submit)
  c) Edit first
```

---

## Step 8 — Submit to JIRA (if requested)

If the user chooses to submit immediately, call the Atlassian MCP:
```json
{
  "cloudId": "{from jira-config.md}",
  "projectKey": "JN",
  "summary": "<epic title>",
  "description": "<formatted description>",
  "issuetype": { "name": "Epic" },
  "assignee": { "accountId": "<assignee>" },
  "priority": { "name": "Medium" },
  "parent": { "key": "JN-XXXX" }
}
```

On success, update the artifact frontmatter with `status: submitted` and `jira_key: JN-XXXX`.

Output:
```
✅ Epic created: JN-XXXX — https://{org}.atlassian.net/browse/JN-XXXX
File: artifacts/initiatives/epics/epic-{slug}.md

Next: Break this epic into Stories using the Story template in guidelines/issue-creation-guidelines.md
```
