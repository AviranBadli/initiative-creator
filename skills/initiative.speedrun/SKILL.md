# Initiative Speedrun Skill

Run the full initiative pipeline — create → review → submit — with minimal interaction. Designed for when you have a clear problem statement and want a JIRA ticket as fast as possible.

All communication and output must be in English.

---

## Input

`$ARGUMENTS` — The problem statement or initiative description. Must be provided. If not provided, ask the user for it before proceeding.

---

## Behavior Philosophy

Speedrun makes reasonable defaults on your behalf and only pauses at two mandatory gates:

1. **After draft generation** — Show the **full draft content** for approval or correction before auto-review runs
2. **Before JIRA submission** — Show the final full draft + JIRA payload for explicit confirmation

Nothing is submitted to JIRA until both approvals are received. Everything else runs automatically.

---

## Step 1 — Load Context

Read these files silently:
- `skills/initiative-template.md`
- `guidelines/initiative-guidelines.md`
- `guidelines/issue-creation-guidelines.md`
- `guidelines/jira-config.md` (for JIRA configuration)

Call `mcp_time_get_current_time` with timezone `Asia/Jerusalem`.

---

## Step 2 — Assess the Input

Evaluate whether `$ARGUMENTS` contains enough information to draft without asking questions.

**Sufficient input includes:** problem description, implied goals, rough scope.

**If input is sufficient:** Skip clarifying questions and go directly to Step 3.

**If input is too vague** (fewer than 3 sentences, no clear problem, no implied goals): Ask a condensed set of 3 questions in a single message:

1. What specific problem does this solve and who is affected?
2. What are the 3 key outcomes that define success?
3. What is explicitly out of scope for now?

Wait for answers, then proceed.

---

## Step 3 — Generate Draft

Apply the full `skills/initiative.create/SKILL.md` logic to generate the initiative draft. Save the artifact to:
```
artifacts/initiatives/initiative-{slug}-{date}.md
```

With frontmatter:
```yaml
---
status: draft
jira_key: ""
created: YYYY-MM-DD
title: ""
assignee: ""
priority: Medium
source_rfes: []
related_initiatives: []
candidate_epics: []
---
```

---

## Step 4 — GATE 1: Full Draft Approval

**Show the entire initiative draft to the user — every section in full. Do NOT summarize.**

```
📄 Draft Generated — Full Content Review
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
File:  {file path}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

# {Title}

{full Context section}

## Goals

{full Goals section — every bullet}

## Scope

{full Scope section — every bullet}

### Out of Scope

{full Out of Scope section — every bullet}

## Expected Impact

{full Expected Impact section}

## Phases & Milestones (if present)

{full Phases section}

## Definition of Done

{full DoD section — every criterion}

## Questions & Answers

{full Q&A section}

## Notes

{full Notes section — all related work links}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Does this look right?
(yes — run auto-review and proceed / or describe what to change)
```

**Do NOT proceed to auto-review until the user says "yes" or equivalent.**

If the user requests changes:
- Apply changes to the draft file on disk
- Re-display the updated full content
- Ask for approval again

---

## Step 5 — Auto-Review

Apply the full `skills/initiative.review/SKILL.md` scoring logic automatically. Do not show the full scoring report — just apply all auto-fixes silently.

If auto-review produces fixes, summarize in one line:
> "Auto-review applied N fixes (title clarity, missing out-of-scope, Q&A date format)."

Update the file frontmatter to `status: reviewed`.

If any Critical items cannot be auto-fixed after 2 cycles → pause and list them explicitly. Wait for the user's input before continuing.

---

## Step 6 — GATE 2: Final Content + JIRA Submission Approval

**Show the final revised content in full, then the JIRA submission details. This is the last chance to review before anything is created in JIRA.**

```
📄 Final Initiative Content (after auto-review)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

# {Title}

{full Context section}

## Goals
{full Goals section}

## Scope
{full Scope section}

### Out of Scope
{full Out of Scope section}

## Expected Impact
{full Expected Impact section}

## Definition of Done
{full DoD section}

## Questions & Answers
{full Q&A section}

## Notes
{full Notes section}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 JIRA Submission Details
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Project:   {project_key} ({project_name from jira-config.md})
Type:      Initiative
Priority:  {priority}
Assignee:  {assignee name from jira-config.md}
Summary:   "{title}"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Create this initiative in JIRA? (yes / no)
```

**Do NOT call any JIRA MCP tool until the user explicitly says "yes".**

If the user says "no": save the draft as-is and tell the user the file path. They can run `/initiative.submit` later.

---

## Step 7 — Submit

After both gates are approved, call the Atlassian MCP `createJiraIssue` with the initiative payload (same as `skills/initiative.submit/SKILL.md` Step 5).

Read `guidelines/jira-config.md` for cloud ID, project key, and default assignee.

On success, update the file frontmatter with `status: submitted` and `jira_key: JN-XXXX`.

Output:
```
✅ Done! Initiative created end-to-end.

JIRA:  JN-XXXX — {jira_base_url}/browse/JN-XXXX
File:  artifacts/initiatives/initiative-{slug}-{date}.md

Next: Run `/initiative.breakdown JN-XXXX` to generate Epics.
```

On failure → show the manual submission guide from `skills/initiative.submit/SKILL.md` Step 6.
