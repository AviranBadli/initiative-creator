# Initiative Status Skill

Pull a live status snapshot of an initiative from JIRA — showing which epics and stories are done, in progress, or blocked — and format it as a concise manager-ready report.

---

## Input

`$ARGUMENTS` — One of:
- A JIRA initiative key (e.g., `JN-3097`)
- Multiple JIRA keys space-separated (e.g., `JN-3097 JN-3112`) — produces a combined report
- Nothing — ask the user which initiative(s) to report on

---

## Step 1 — Load Configuration

Read `guidelines/jira-config.md` for cloud ID and project key.
Call `mcp_time_get_current_time` with timezone `Asia/Jerusalem`. Store the date — it appears in the report header.

---

## Step 2 — Fetch Initiative and All Children

For each provided initiative key, run these queries in parallel:

```
# Fetch the initiative itself
jira_get_issue(issue_key: "JN-XXXX", expand: "changelog,comments")

# Fetch all epics under this initiative
jira_search_issues(jql: 'project = JN AND issuetype = Epic AND "Initiative Link" = JN-XXXX ORDER BY status ASC')

# Fetch all stories (in case epics are missing or stories link directly)
jira_search_issues(jql: 'project = JN AND issuetype = Story AND "Epic Link" in (epics_found_above) ORDER BY status ASC')
```

Also check for any issues with `label = blocked` or `priority = Critical` that are linked to this initiative.

---

## Step 3 — Classify Every Issue

For each Epic and Story, classify into one of these buckets:

| Bucket | JIRA statuses that map here |
|---|---|
| ✅ Done | Done, Closed, Resolved, Won't Fix |
| 🔄 In Progress | In Progress, In Review, In Testing, Under Review |
| ⏳ Not Started | To Do, Open, Backlog, New |
| 🚨 Blocked | Any status with "blocked" label, or "Blocked" status |
| ❌ Cancelled | Cancelled, Won't Do |

---

## Step 4 — Calculate Progress Metrics

For each initiative:

```
Epics:   {done}/{total} complete ({percent}%)
Stories: {done}/{total} complete ({percent}%)

Velocity estimate:
  Sprints elapsed: {N}
  Stories completed per sprint: {avg}
  Stories remaining: {N}
  Estimated sprints to completion: {N} (at current velocity)
```

---

## Step 5 — Generate Status Report

```
📊 Initiative Status Report
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
As of: {today's date and time}

Initiative: "{title}"
JIRA: JN-XXXX | Status: {status} | Priority: {priority}
Assignee: {assignee}

PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Epics:   [{done}/{total}] ████████░░ {percent}%
Stories: [{done}/{total}] ██████░░░░ {percent}%

EPICS BREAKDOWN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ DONE ({N} epics):
  • JN-XXX "{Epic title}" — {done_stories}/{total_stories} stories

🔄 IN PROGRESS ({N} epics):
  • JN-XXX "{Epic title}" — {done_stories}/{total_stories} stories
    Active stories: "{story title}" (assignee), "{story title}" (assignee)

⏳ NOT STARTED ({N} epics):
  • JN-XXX "{Epic title}" — {total_stories} stories queued

🚨 BLOCKED ({N} epics/stories):
  • JN-XXX "{Epic or story title}" — {blocker description if in comments}

VELOCITY ESTIMATE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Stories done/sprint (avg): {N}
Stories remaining: {N}
Est. sprints to completion: ~{N}

RECENT ACTIVITY (last 14 days)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
• {date}: "{story}" moved to Done by {person}
• {date}: "{story}" moved to In Progress by {person}
• {date}: Comment by {person}: "{key insight from comment}"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

If multiple initiatives were requested, repeat the above block for each and add a combined summary at the top:

```
🗂 Multi-Initiative Summary
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
JN-3097 "..." ████████░░ 78% | 🚨 1 blocked
JN-3112 "..." ████░░░░░░ 42% | ✅ on track
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Step 6 — Highlight Action Items

After the report, surface any items that require attention:

```
⚡ Action Items
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
{If blocked items exist:}
🚨 BLOCKED: JN-XXX "{title}" — {who should unblock / what decision is needed}

{If velocity is low:}
⚠️  At current pace ({N} stories/sprint), this initiative will take ~{N} more sprints.
   Consider: scope reduction, additional resourcing, or timeline adjustment.

{If any epic has 0 active stories despite being In Progress:}
⚠️  Epic JN-XXX "{title}" is marked In Progress but has no active stories assigned.

{If nothing to flag:}
✅ No blockers or risks identified. Initiative is on track.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Arguments

`$ARGUMENTS` — JIRA initiative key(s) or leave blank to be prompted.
