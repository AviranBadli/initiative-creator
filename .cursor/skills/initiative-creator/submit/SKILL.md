# Initiative Submit Skill

Submit a reviewed initiative draft to JIRA as an Initiative issue type via the Atlassian MCP. Always show the full payload for user approval before creating anything.

---

## Input

`$ARGUMENTS` — One of:
- A file path to a local reviewed draft (e.g., `artifacts/initiatives/initiative-unified-logging-2026-03-29.md`)
- Nothing — the skill will find the most recently modified file in `artifacts/initiatives/` with `status: reviewed`

---

## Step 1 — Load the Draft

Read the specified file (or auto-detect the most recent `status: reviewed` file).

If the file has `status: draft` (not yet reviewed), warn the user:

> ⚠️ This initiative has not been reviewed yet. It is strongly recommended to run `/initiative.review` first to catch quality issues. Do you want to proceed with submission anyway, or run a review first?

Wait for the user's response before proceeding.

If `status: submitted` and `jira_key` is already set, warn the user:

> ⚠️ This initiative was already submitted as {jira_key}. Do you want to update the existing issue, or create a new one?

---

## Step 2 — Build the JIRA Payload

Extract the following fields from the initiative file:

| JIRA Field | Source |
|---|---|
| `summary` | Title line (first H1, strip the `#`) |
| `description` | Everything below the title, formatted as Atlassian Document Format (ADF) or plain text |
| `issuetype` | Always `{ "name": "Initiative" }` |
| `project` | From `guidelines/jira-config.md` → `project_key` |
| `assignee` | From frontmatter `assignee` field, or default from `guidelines/jira-config.md` |
| `priority` | From frontmatter `priority` field, default: `{ "name": "Medium" }` |

### JIRA Configuration

Read `guidelines/jira-config.md` before submitting to get:
- **Cloud ID** — your Atlassian cloud identifier
- **Project Key** — your JIRA project (e.g., `JN`, `RHAIRFE`)
- **Default Assignee** — JIRA account ID
- **Issue Type:** Always `Initiative`

If `guidelines/jira-config.md` does not exist, ask the user for these values before proceeding.

### Description Formatting

Convert the markdown initiative body to plain text with Jira-compatible formatting:
- `##` headers → `h2.` prefix in Jira wiki markup
- `**bold**` → `*bold*`
- Bullet lists → `*` prefix
- Numbered lists → `#` prefix
- Code blocks → `{code}...{code}`

---

## Step 3 — Show Approval Preview

ALWAYS show the full payload to the user before creating anything. Format:

```
📋 Initiative Submission Preview
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Project:    {project_key} ({project_name})
Type:       Initiative
Priority:   {priority}
Assignee:   {name} ({email})

Summary:
  "{title}"

Description (first 300 chars):
  "{description_excerpt}..."

Full description: {character_count} characters

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Shall I create this initiative in JIRA? (yes / no / edit first)
```

Wait for explicit user confirmation. Do NOT proceed without a clear "yes" or equivalent.

---

## Step 4 — Submit to JIRA

After user confirmation, call the Atlassian MCP `createJiraIssue` tool:

```json
{
  "cloudId": "511393e2-4fb8-40dd-aa81-0b7818a52bfc",
  "projectKey": "JN",
  "summary": "<title from file>",
  "description": "<formatted description>",
  "issuetype": { "name": "Initiative" },
  "assignee": { "accountId": "<assignee from frontmatter>" },
  "priority": { "name": "<priority from frontmatter>" }
}
```

---

## Step 5 — Handle Response

### On Success

Extract the new JIRA issue key (e.g., `JN-3150`) from the response.

1. Update the artifact file frontmatter:
```yaml
status: submitted
jira_key: JN-XXXX
```

2. Tell the user:

```
✅ Initiative created successfully!

JIRA Key:  JN-XXXX
Link:      https://jounce.atlassian.net/browse/JN-XXXX
File:      artifacts/initiatives/initiative-{slug}-{date}.md

Next steps:
- Run `/initiative.breakdown JN-XXXX` to generate Epic drafts under this initiative
- Share the JIRA link with your team for alignment
```

### On Failure

If the MCP call fails:

```
⚠️ JIRA submission failed.

Error: {error message}

The draft is saved at: {file path}

Manual submission guide:
1. Go to https://jounce.atlassian.net/jira/software/projects/JN/boards
2. Click "Create Issue" → Type: Initiative
3. Paste the content from {file path}
4. Set Priority: {priority}, Assignee: {name}
```

Do NOT retry automatically. Show the manual path and stop.
