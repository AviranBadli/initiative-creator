# Initiative Split Skill

Decompose an oversized initiative into two or more right-sized initiatives, each with clear scope boundaries and no coverage gaps.

---

## When to Use This Skill

Use this skill when an initiative is:
- Covering too many problem areas (5+ distinct goals that don't share a common thread)
- Too large for a single team to own end-to-end
- Spanning more than 6-8 sprints with no natural phase boundary
- Failing review because scope is unfocused or DoD is too broad

---

## Input

`$ARGUMENTS` — One of:
- A JIRA initiative key (e.g., `JN-3097`) — fetches the initiative from JIRA
- A file path to a local initiative artifact
- Nothing — auto-detect the most recently modified initiative in `artifacts/initiatives/`

---

## Step 1 — Load and Analyze

Read silently:
- `skills/initiative-template.md`
- `guidelines/initiative-guidelines.md`
- `guidelines/issue-creation-guidelines.md`

Call `mcp_time_get_current_time` with timezone `Asia/Jerusalem`.

Load the initiative (from JIRA or file). Read it carefully — multiple times — focusing on:
1. How many distinct problem areas / user needs are addressed?
2. Are there natural phase boundaries where one piece of work clearly unlocks another?
3. Which goals, scope items, and DoD criteria cluster together?
4. Which parts could be owned by different people or teams?

---

## Step 2 — Propose Split to User

Present a proposed split before doing any work:

```
📐 Initiative Split Proposal
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Original: "{original title}" ({JN-key or file})

Proposed Split into {N} initiatives:

Initiative A: "{proposed title A}"
  Covers: {scope items from original that belong here}
  Rationale: {why these items belong together}

Initiative B: "{proposed title B}"
  Covers: {scope items from original that belong here}
  Rationale: {why these items belong together}

[Initiative C if needed...]

Coverage check: {confirm all original scope items are assigned to exactly one new initiative}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Does this split look right? (yes / adjust / split differently)
```

Wait for user confirmation or adjustment before proceeding.

---

## Step 3 — Draft Each New Initiative

For each new initiative, produce a complete draft using the structure from `skills/initiative-template.md`.

**Key rules for split initiatives:**
- Each new initiative must have its own self-contained Context — do not assume the reader has seen the other initiative
- Goals must be scoped to only what this initiative covers
- Out of Scope must explicitly call out the other initiative(s) by name/area to prevent confusion
- DoD must be achievable without completing the sibling initiatives
- Notes section must reference the original initiative and the sibling initiative(s)

**Notes format for split initiatives:**
```
## Notes

**Split from:** [{original JN-key or title}]({link}) — this initiative was created by splitting the original
**Sibling initiative:** [{other initiative title}]({link or "see artifact"}) — covers the other half of the original scope

**Original source RFEs (if any):**
* [RHAIRFE-XXX] — assigned to this initiative
* [RHAIRFE-YYY] — assigned to sibling initiative
```

---

## Step 4 — Save Artifacts

Save each new initiative to:
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
split_from: ""          # original JN key or file slug
sibling_initiatives: [] # slugs or JN keys of the other split initiatives
source_rfes: []
related_rfes: []
related_initiatives: []
candidate_epics: []
---
```

If the original initiative was a JIRA ticket, mark its description with a note that it has been split (but do NOT modify the original JIRA ticket automatically — ask the user if they want to update it).

---

## Step 5 — Coverage Verification

After drafting, run a coverage check:

For every scope item in the original initiative → verify it appears in exactly one new initiative's scope.
For every DoD criterion in the original → verify it is captured in exactly one new initiative's DoD.
For every source RFE in the original → verify it is assigned to exactly one new initiative.

If any item is uncovered or duplicated → fix before presenting results.

---

## Step 6 — Output

```
✅ Split Complete

Original initiative split into {N} new drafts:

1. [{Initiative A title}](artifacts/initiatives/initiative-{slug-a}-{date}.md)
   Goals: {N} | DoD criteria: {N} | Source RFEs: {N}

2. [{Initiative B title}](artifacts/initiatives/initiative-{slug-b}-{date}.md)
   Goals: {N} | DoD criteria: {N} | Source RFEs: {N}

Coverage: ✅ All original scope items assigned | ✅ All DoD criteria covered | ✅ All RFEs assigned

Next steps:
- Review each draft with /initiative.review
- Submit each with /initiative.submit
- Ask your team: should the original {JN-key} be closed/linked to the new ones?
```
