# Initiative Speedrun Skill

Run the full initiative pipeline — create → review → submit — with minimal interaction. Designed for when you have a clear problem statement and want a JIRA ticket as fast as possible.

---

## Input

`$ARGUMENTS` — The problem statement or initiative description. Must be provided. If not provided, ask the user for it before proceeding.

---

## Behavior Philosophy

Speedrun differs from running the individual skills sequentially in one key way: **it makes reasonable defaults on your behalf** and only pauses for things it truly cannot decide alone. The two mandatory pauses are:

1. **After draft generation** — Show the draft for quick approval or correction before reviewing
2. **Before JIRA submission** — Show the full JIRA payload for explicit confirmation

Everything else runs automatically.

---

## Step 1 — Load Context

Read these files silently:
- `skills/initiative-template.md`
- `guidelines/initiative-guidelines.md`
- `guidelines/issue-creation-guidelines.md`

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

Apply the full `create/SKILL.md` logic to generate the initiative draft. Save the artifact to:
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
assignee: "712020:f6dc72f9-3c15-4b11-82f2-298a7f78d125"
priority: Medium
---
```

---

## Step 4 — Mandatory Pause 1: Draft Approval

Present the draft to the user in a condensed format:

```
📝 Draft Generated — Quick Review
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Title:  "{title}"
File:   {file path}

Goals:
{bullet list of goals}

Definition of Done:
{bullet list of DoD criteria}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Looks good? (yes to continue / or describe what to change)
```

If the user says "yes" or equivalent → proceed to Step 5.
If the user requests changes → apply them to the draft file, then re-present and ask again.

---

## Step 5 — Auto-Review

Apply the full `review/SKILL.md` scoring logic automatically. Do not present the full scoring report — just apply all auto-fixes silently.

If auto-review produces fixes, summarize them in one line:
> "Auto-review applied 3 fixes (title clarity, missing out-of-scope, Q&A date format)."

Update the file frontmatter to `status: reviewed`.

If any Critical items cannot be auto-fixed after 2 cycles → pause and list them for the user. Wait for their input before continuing.

---

## Step 6 — Mandatory Pause 2: Submit Confirmation

Apply the full `submit/SKILL.md` payload preview. Show:

```
📋 Ready to Submit to JIRA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Project:    JN (Jounce)
Type:       Initiative
Priority:   Medium
Assignee:   Aviran Badli (abadli@redhat.com)

Summary:
  "{title}"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Create this initiative in JIRA? (yes / no)
```

Wait for explicit "yes" before submitting.

---

## Step 7 — Submit

Call the Atlassian MCP `createJiraIssue` with the initiative payload (same as `submit/SKILL.md` Step 4).

On success, update the file frontmatter with `status: submitted` and `jira_key: JN-XXXX`.

Output:
```
✅ Done! Initiative created end-to-end.

JIRA:  JN-XXXX — https://jounce.atlassian.net/browse/JN-XXXX
File:  artifacts/initiatives/initiative-{slug}-{date}.md

Next: Run `/initiative.breakdown JN-XXXX` to generate Epics.
```

On failure → show the manual submission guide from `submit/SKILL.md` Step 5.
