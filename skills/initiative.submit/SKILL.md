# Initiative Submit Skill

Submit a reviewed initiative draft to JIRA as an Initiative issue type via the Atlassian MCP.

**Always show the full draft content first and get explicit approval. Then show the JIRA payload and get a second explicit approval. Nothing is created in JIRA until both approvals are received.**

All communication and output must be in English.

---

## Input

`$ARGUMENTS` — One of:
- A file path to a local reviewed draft (e.g., `artifacts/initiatives/initiative-unified-logging-2026-03-29.md`)
- Nothing — the skill will find the most recently modified file in `artifacts/initiatives/` with `status: reviewed`

---

## Step 1 — Load the Draft

Read the specified file (or auto-detect the most recent `status: reviewed` file).

Read `guidelines/jira-config.md` to load JIRA configuration.

If the file has `status: draft` (not yet reviewed), warn the user:

> ⚠️ This initiative has not been reviewed yet. It is strongly recommended to run `/initiative.review` first to catch quality issues.
> Do you want to proceed with submission anyway, or run a review first?

Wait for the user's response before proceeding.

If `status: submitted` and `jira_key` is already set, warn the user:

> ⚠️ This initiative was already submitted as {jira_key}. Do you want to update the existing issue (use `/initiative.update`), or create a new one?

---

## Step 2 — GATE 1: Show Full Draft for Content Approval

**Before building any JIRA payload, show the entire initiative draft to the user.**

Display every section in full — do NOT summarize or truncate:

```
📄 Initiative Draft — Full Content Review
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
File:   {file path}
Status: {status}
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

## Phases & Milestones (if present)

{full Phases section}

## Definition of Done

{full DoD section}

## Questions & Answers

{full Q&A section}

## Notes

{full Notes section}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Does this content look correct and complete?
(yes — proceed to JIRA submission / no — describe what to change / edit — I will edit the file)
```

**Do NOT proceed until the user explicitly says "yes" or an equivalent approval.**

If the user requests changes:
- Apply the changes to the draft file on disk
- Re-display the updated full content
- Ask for approval again

If the user says "edit": tell them the file path and wait for them to confirm they are done editing before re-reading and displaying the updated content.

---

## Step 3 — Build the JIRA Payload

Only after Gate 1 is approved, build the submission payload.

Extract the following fields from the initiative file:

| JIRA Field | Source |
|---|---|
| `summary` | Title line (first H1, strip the `#`) |
| `description` | Everything below the title, formatted as Jira wiki markup |
| `issuetype` | Always `{ "name": "Initiative" }` |
| `project` | From `guidelines/jira-config.md` → `project_key` |
| `assignee` | From frontmatter `assignee` field, or default from `guidelines/jira-config.md` |
| `priority` | From frontmatter `priority` field, default: `{ "name": "Medium" }` |

### JIRA Configuration

Read `guidelines/jira-config.md` for:
- **Cloud ID** — your Atlassian cloud identifier
- **Project Key** — your JIRA project (e.g., `JN`, `RHAIRFE`)
- **Default Assignee** — JIRA account ID

If `guidelines/jira-config.md` does not exist, ask the user for these values before proceeding.

### Description Formatting

Convert the markdown body to Jira wiki markup:
- `##` headers → `h2.` prefix
- `**bold**` → `*bold*`
- Bullet lists → `*` prefix
- Numbered lists → `#` prefix
- Code blocks → `{code}...{code}`

---

## Step 4 — GATE 2: JIRA Payload Approval

**Second approval gate. Show the JIRA submission metadata and ask again.**

```
📋 JIRA Submission Details — Final Confirmation
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Project:    {project_key} ({project_name})
Type:       Initiative
Priority:   {priority}
Assignee:   {name} ({email})
Cloud:      {jira_base_url}

Summary (ticket title):
  "{title}"

Description length: {character_count} characters

Source RFEs:  {RHAIRFE-XXX, ... or "none"}
Related:      {JN-XXX, ... or "none"}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Create this initiative in JIRA now? (yes / no)
```

**Do NOT call any JIRA MCP tool until the user explicitly says "yes".**

If the user says "no" at this stage: stop and leave the draft file as-is. The user can re-run `/initiative.submit` when ready.

---

## Step 5 — Submit to JIRA

After both gates are approved, call the Atlassian MCP `createJiraIssue`:

```json
{
  "cloudId": "{cloud_id from jira-config.md}",
  "projectKey": "{project_key from jira-config.md}",
  "summary": "{title from file}",
  "description": "{formatted description}",
  "issuetype": { "name": "Initiative" },
  "assignee": { "accountId": "{assignee from frontmatter or config}" },
  "priority": { "name": "{priority from frontmatter}" }
}
```

---

## Step 6 — Handle Response

### On Success

Extract the new JIRA issue key from the response.

Update the artifact file frontmatter:
```yaml
status: submitted
jira_key: JN-XXXX
```

Output:
```
✅ Initiative created successfully!

JIRA Key:  JN-XXXX
Link:      {jira_base_url}/browse/JN-XXXX
File:      {artifact file path}

Next steps:
- Run `/initiative.breakdown JN-XXXX` to generate Epic drafts under this initiative
- Share the JIRA link with your team for alignment
```

### On Failure

```
⚠️ JIRA submission failed.

Error: {error message}

The draft is saved at: {file path}

Manual submission:
1. Go to {jira_base_url}/jira/software/projects/{project_key}/boards
2. Click "Create Issue" → Type: Initiative
3. Copy the content from {file path}
4. Set Priority: {priority}, Assignee: {name}
```

Do NOT retry automatically. Show the manual path and stop.
