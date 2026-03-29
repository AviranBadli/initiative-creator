---
name: initiative.submit
description: "Submit a reviewed initiative to JIRA via Atlassian MCP with full approval gate. Accepts a file path or auto-detects latest reviewed draft."
user-invocable: true
allowed-tools:
  - mcp__atlassian__jira_create_issue
  - mcp__time__get_current_time
  - Read
  - Write
---

Read the full contents of `skills/initiative.submit/SKILL.md` and follow all instructions exactly.

$ARGUMENTS
