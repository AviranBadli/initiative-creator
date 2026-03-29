---
name: initiative.update
description: "Fetch an existing JIRA initiative, apply edits (scope, DoD, Q&A, phases), and re-submit the updated version to the same ticket key."
user-invocable: true
allowed-tools:
  - mcp__atlassian__jira_get_issue
  - mcp__atlassian__jira_update_issue
  - mcp__time__get_current_time
  - Read
  - Write
---

Read the full contents of `skills/initiative.update/SKILL.md` and follow all instructions exactly.

$ARGUMENTS
