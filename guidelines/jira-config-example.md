# JIRA Configuration — Example

Copy this file to `guidelines/jira-config.md` and fill in your team's values.
`jira-config.md` is gitignored so your account IDs stay private.

---

## Atlassian Cloud

```
cloud_id: YOUR_ATLASSIAN_CLOUD_ID
```

Find your Cloud ID at: `https://YOUR_ORG.atlassian.net/_edge/tenant_info`

## JIRA Project

```
project_key: YOUR_PROJECT_KEY
project_name: Your Project Name
jira_base_url: https://YOUR_ORG.atlassian.net
```

## Default Issue Settings

```
default_assignee_account_id: YOUR_JIRA_ACCOUNT_ID
default_priority: Medium
```

Find your account ID at: `https://YOUR_ORG.atlassian.net/rest/api/3/myself`

## Team Members (for Epic assignment)

```
team:
  - name: Team Member 1
    account_id: JIRA_ACCOUNT_ID_1
    email: member1@company.com
  - name: Team Member 2
    account_id: JIRA_ACCOUNT_ID_2
    email: member2@company.com
```

---

## How Skills Use This File

The `submit` and `speedrun` skills read this file to:
1. Set the `cloudId` for all MCP calls
2. Set the `project.key` for ticket creation
3. Fall back to `default_assignee_account_id` when no assignee is in the artifact frontmatter
4. Build the JIRA browse URL: `{jira_base_url}/browse/{jira_key}`
