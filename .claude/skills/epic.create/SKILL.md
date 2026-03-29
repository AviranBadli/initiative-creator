---
name: epic.create
description: "Create a single well-structured JIRA Epic under a specified initiative. Checks for overlap with existing epics and optionally submits to JIRA immediately."
user-invocable: true
allowed-tools:
  - mcp__atlassian__jira_get_issue
  - mcp__atlassian__jira_search_issues
  - mcp__atlassian__jira_create_issue
  - mcp__time__get_current_time
  - Read
  - Write
---

Read the full contents of `skills/epic.create/SKILL.md` and follow all instructions exactly.

$ARGUMENTS
