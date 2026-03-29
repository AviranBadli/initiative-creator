# Initiative Update Skill

Fetch an existing JIRA initiative, apply edits (description, goals, scope, DoD, Q&A), and re-submit the updated version — keeping the same ticket key.

---

## When to Use This Skill

Use this skill when:
- An initiative's scope has changed after it was submitted to JIRA
- You need to add answered Q&A entries, new open questions, or notes
- Goals or DoD need to be refined based on new information
- A Phase has been completed and the initiative needs to reflect new state

---

## Input

`$ARGUMENTS` — A JIRA initiative key (e.g., `JN-3097`). Required — this skill always works on an existing JIRA ticket.

---

## Step 1 — Fetch Current Initiative from JIRA

Read `guidelines/jira-config.md` for cloud ID.

Fetch the issue using the Atlassian MCP:
```
jira_get_issue(issue_key: "JN-XXXX", expand: "changelog,comments")
```

Extract:
- Summary (title)
- Full description body
- Status, assignee, priority
- All comments (may contain decisions, Q&A answers, new questions)
- Changelog (to understand what has changed recently)

Call `mcp_time_get_current_time` with timezone `Asia/Jerusalem`.

---

## Step 2 — Parse and Reconstruct

Convert the JIRA description back into structured markdown using the `skills/initiative-template.md` section structure. Identify all existing sections and their current content.

Save a local working copy at:
```
artifacts/initiatives/initiative-{JN-key}-update-{date}.md
```

With frontmatter:
```yaml
---
status: update-draft
jira_key: JN-XXXX
created: YYYY-MM-DD   # original creation date from JIRA
updated: YYYY-MM-DD   # today
title: ""
assignee: ""
priority: ""
source_rfes: []
related_initiatives: []
---
```

---

## Step 3 — Scan Comments for Pending Updates

Read all JIRA comments on the ticket. Look for:
- Answered questions (move from Open to Answered Q&A)
- New scope decisions or blockers mentioned
- Status changes or phase completions noted in comments
- New dependencies identified

Present a summary to the user:

```
📋 Found in Comments
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
{N} comments found. Suggested updates from comments:

→ Q&A: "{question}" was answered by {person} on {date}: "{answer}"
→ Scope note: {person} mentioned "{new constraint or decision}"
→ New open question raised: "{question}"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Should I apply these to the initiative description? (yes / pick specific ones / no)
```

---

## Step 4 — Ask What to Update

After presenting the comment-derived suggestions, ask:

```
What else needs to be updated? You can say things like:

- "Add a new goal: ..."
- "Remove scope item: ..."
- "Update DoD — criterion 2 is now complete"
- "Add open question: ..."
- "Change priority to High"
- "Add Phase 3: ..."
- "Update context — we learned that ..."
```

Collect all requested changes. If nothing further, proceed with only the comment-derived updates.

---

## Step 5 — Apply All Changes

Apply every change to the local working copy:

**Q&A updates:**
- Move answered questions from Open → Answered, add answer + date + "Answered by"
- Add new Open Questions with today's date

**Scope updates:**
- Add/remove scope items as requested
- Update Out of Scope if needed

**DoD updates:**
- Mark completed criteria with ~~strikethrough~~ and a "(Completed {date})" note, OR remove them
- Add new criteria

**Context updates:**
- Add a new paragraph or update existing paragraphs with new information

**Phase updates:**
- Mark completed phases, add new phases if needed

---

## Step 6 — Show Diff and Confirm

Present a clear before/after summary of every change:

```
📝 Proposed Changes to JN-XXXX
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Q&A (2 changes):
  ✅ Moved to Answered: "Should we use OpenSearch or Splunk?"
     → Answer: "OpenSearch — decided by Mark on {date}"
  ➕ New Open Question: "What is the log retention policy?"

Scope (1 change):
  ➕ Added: "Support for async log streaming via Kafka"

DoD (1 change):
  ➕ Added: "Log retention policy is defined and documented"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Apply these changes and update JN-XXXX in JIRA? (yes / adjust / no)
```

Wait for explicit user confirmation before submitting.

---

## Step 7 — Submit Update to JIRA

After confirmation, call the Atlassian MCP to update the issue:
```
jira_update_issue(
  issue_key: "JN-XXXX",
  summary: "<updated title if changed>",
  description: "<full updated description in Jira format>"
)
```

Update the local artifact frontmatter:
```yaml
status: submitted
updated: YYYY-MM-DD
```

---

## Step 8 — Output

```
✅ Initiative Updated

JIRA:  JN-XXXX — https://{org}.atlassian.net/browse/JN-XXXX
File:  artifacts/initiatives/initiative-JN-XXXX-update-{date}.md

Changes applied:
  Q&A:   {N} answered, {N} new questions added
  Scope: {N} items added/removed
  DoD:   {N} criteria updated
  Other: {summary}
```
