---
name: initiative.create
description: "Generate a complete JIRA Initiative draft from a problem statement with current-state discovery and RFE linkage"
user-invocable: true
allowed-tools:
  - mcp__atlassian__jira_search_issues
  - mcp__atlassian__jira_get_issue
  - mcp__atlassian__jira_create_issue
  - mcp__time__get_current_time
  - Read
  - Write
---

# Initiative Create Skill

Generate a complete, structured JIRA Initiative draft from a problem statement. Discover existing related work first, ask deep clarifying questions, then produce a fully-formed artifact file.

---

## Step 1 — Load Context

Before asking any questions, read these files silently to internalize all rules and templates:

1. `.claude/skills/initiative-template.md` — canonical section structure
2. `guidelines/initiative-guidelines.md` — writing rules, DoD requirements, best practices
3. `guidelines/issue-creation-guidelines.md` — checklist and quality bar

Do NOT skip this step. These files define what "complete" means for an initiative.

---

## Step 2 — Get Current Date

Call `mcp_time_get_current_time` with timezone `Asia/Jerusalem` immediately. Store the result — you will use it for all timestamps in the Q&A section and the artifact frontmatter.

---

## Step 3 — Current State Discovery (JIRA + Existing Work)

**This step runs automatically before asking the user anything.** Search for existing work that is related to the initiative topic.

### 3a — Search JIRA for Related Initiatives

Use the Atlassian MCP to search for existing initiatives. Read `guidelines/jira-config.md` first to get the project key and cloud ID, then run these JQL queries in parallel:

```
# Find existing initiatives in the project
issuetype = Initiative AND project = {PROJECT_KEY} ORDER BY created DESC

# Find initiatives mentioning the topic keywords
issuetype = Initiative AND project = {PROJECT_KEY} AND text ~ "{keyword_from_topic}" ORDER BY updated DESC
```

Extract all results: key, summary, status, assignee.

### 3b — Search JIRA for Related Epics

```
# Find open epics that might belong under this initiative
issuetype = Epic AND project = {PROJECT_KEY} AND text ~ "{keyword_from_topic}" ORDER BY updated DESC

# Find epics with no parent initiative
issuetype = Epic AND project = {PROJECT_KEY} AND "Epic Link" is EMPTY AND text ~ "{keyword_from_topic}"
```

### 3c — Search for Related RFEs

RFEs (Requests for Enhancement) in the `RHAIRFE` JIRA project are often the origin of initiatives — a group of related RFEs gets bundled into one initiative. Search for them in parallel with the other queries:

```
# RFEs mentioning the topic keywords (RHAIRFE project)
issuetype in (RFE, "Feature Request") AND project = RHAIRFE AND text ~ "{keyword_from_topic}" ORDER BY updated DESC

# RFEs that are already approved / accepted
issuetype in (RFE, "Feature Request") AND project = RHAIRFE AND text ~ "{keyword_from_topic}" AND status in (Accepted, Approved, "In Progress") ORDER BY updated DESC
```

Extract for each RFE: key, summary, status, priority, assignee, and the first 200 characters of the description.

If the RHAIRFE project key is not available in `guidelines/jira-config.md`, try `RHAIRFE` as the default. If this project does not exist in the connected JIRA instance, skip 3c and note it.

### 3d — Check Local Artifacts

Scan `artifacts/initiatives/` for any existing draft or submitted initiative files that match the topic. Read their titles and status from frontmatter.

### 3e — Present Discovery Results to User

After all searches complete, present a concise summary — **this is the first thing the user sees**:

```
🔍 Current State Discovery
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Existing Initiatives found ({N} total):
  • {JN-XXX} "{Title}" — Status: {status}
  • {JN-XXX} "{Title}" — Status: {status}
  [or: No related initiatives found]

Potentially Related Epics ({N} found):
  • {JN-XXX} "{Title}" — Status: {status}, Parent: {initiative or none}
  [or: No related epics found]

Related RFEs found ({N} total):
  • {RHAIRFE-XXX} "{Title}" — Status: {status}, Priority: {priority}
    "{first 100 chars of description}..."
  • {RHAIRFE-XXX} "{Title}" — Status: {status}, Priority: {priority}
    "{first 100 chars of description}..."
  [or: No related RFEs found]

Local Draft Artifacts:
  • {file path} — status: {draft/reviewed/submitted}
  [or: No local drafts found]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

If JIRA search is unavailable (MCP not configured), skip 3a, 3b, and 3c, note it, and continue.

---

## Step 4 — Current State Questions

Ask ALL of the following questions in a single message. These questions are specifically designed to surface existing work, adjacent initiatives, and gaps before drafting. Do NOT split into multiple rounds.

**Present them exactly like this:**

---

Before I draft the initiative, I need to understand the current state deeply. Please answer as many of these as you can:

**About what already exists:**
1. What is the current state of this area today? What already works, what is broken, what is missing?
2. Are there any existing JIRA initiatives or epics (beyond what I found above) that overlap with this topic?
3. Is there existing work already in progress that this initiative would build on, replace, or be blocked by?
4. Have we tried to solve this before? If yes, what happened and what did we learn?

**About related work that should be included:**
5. Are there epics or stories that are currently "floating" (no parent initiative) that should live under this initiative?
6. Are there adjacent problem areas that are closely related — that we might want to include in scope or at minimum reference as "related"?
7. Is there work happening on other teams or in other initiatives that this depends on or affects?

**About RFEs this initiative is based on:**
8. Is this initiative based on one or more RFEs? If yes, which ones (RHAIRFE-XXX numbers)?
   _(From the RFEs I found above — are any of them the source for this initiative? Or are there other RFEs I didn't surface?)_
9. If based on RFEs: should this initiative implement ALL of those RFEs, or only a subset? Which ones are out of scope for now?

**About the initiative boundaries:**
10. What parts of this problem space are explicitly owned by another initiative already? (helps define out-of-scope)
11. Are there architectural decisions, design docs, or tech specs already made that this initiative must respect?
12. What would a "Phase 2" of this initiative look like? (helps define what goes in Phase 1 vs. what is future work)

---

After the user responds:

**If RFEs are confirmed (questions 8-9):**
1. Fetch the full content of each confirmed RFE using the Atlassian MCP (`get_issue` with `expand: "description,comments"`)
2. Read their descriptions carefully — extract: problem statement, customer impact, requested behavior, acceptance criteria
3. Store this as **RFE Content Summary** (internal) — you will use it to populate the initiative's Context, Goals, and Expected Impact sections

**In all cases**, synthesize answers into a **Related Work Summary** (internal, not shown to user) covering:
- Which RHAIRFE keys are the source RFEs for this initiative (if any)
- Which existing JN tickets should be listed as related in the Notes section
- Which epics are candidates to be children of this initiative
- What "out of scope" items are clearly defined by adjacent initiatives or excluded RFEs
- What prior decisions or constraints to mention in Context

---

## Step 5 — Clarifying Questions (Initiative Shape)

Now ask the questions needed to define the initiative itself. **Combine with any unanswered items from Step 4 into a single message** — never ask two rounds of questions separately.

1. **Problem:** What specific problem or pain point does this initiative solve? Who is currently affected and how?
2. **Goals:** What are the 3-6 key outcomes that define success? Try to make them measurable.
3. **Scope:** What is explicitly IN scope for this initiative? What are you intentionally leaving OUT for later?
4. **Definition of Done:** How will you know this initiative is 100% complete? What can you verify?
5. **Timeline / Phases:** Is this single-phase or multi-phase? Roughly how many sprints do you expect?
6. **Team:** Who are the key people involved? Any external teams or stakeholders to name?

Skip any question the user already answered in Step 4 or in their original description.

---

## Step 6 — Draft the Initiative

Using answers from Steps 4 and 5 plus the discovery results from Step 3, produce a complete initiative draft.

### Section Requirements

Follow the structure from `initiative-template.md` exactly. Every section below is **required**:

#### Title
- Format: `[Action Verb] [What] [Purpose/Outcome]`
- 60-80 characters max
- Examples: "Allow users to run LLM benchmarks easily and intuitively", "Build unified logging system for JBenchmark"
- Must NOT be vague (no "Improve X" or "Work on Y")

#### Context (2-4 paragraphs)
- **Current state paragraph** — describe the situation TODAY based on what the user told you in Step 4 questions 1-4. Be specific: what exists, what is broken, what has been tried.
- **RFE origin paragraph** _(only if source RFEs exist)_ — briefly state that this initiative was created to address one or more customer/field RFEs. Summarize the core customer need expressed in those RFEs in 1-2 sentences. Do NOT copy-paste RFE text verbatim — synthesize the customer intent.
- **Why this matters now** — urgency, strategic alignment, what unblocks
- **Who is affected** — teams, users, customers
- **Business or technical drivers**

**If source RFEs were fetched:** Use the problem statements and customer impact sections from the RFE Content Summary to make the Context more specific and grounded. The initiative context should feel like a natural continuation of the RFE problem space.

#### Goals (3-6 bullets)
- Specific and measurable
- Outcome-focused, not activity-focused
- Bold key terms for scannability
- **If source RFEs exist:** at least one goal should directly address the RFE acceptance criteria or requested behavior. Phrase it as an initiative goal, not an RFE description.

#### Scope
- Bullet list of what is included
- Sub-section "Out of Scope" — use the boundary information from Step 4 questions 8-9 to make this precise and concrete

#### Expected Impact
- Quantifiable where possible
- Connect to business outcomes

#### Phases & Milestones _(only if multi-phase)_
- Include only if the initiative spans multiple distinct phases
- Phase 1 = current scope; Phase 2 = the future work identified in Step 4 question 10
- Each phase gets a name, 2-3 sentence description, and deliverables list

#### Definition of Done
- Verifiable, specific criteria — not "improve" but "reduce by X%" or "all Y are complete"
- 3-6 bullet points minimum
- A reader should be able to check each item off with a yes/no

#### Questions & Answers
- Always include this section
- Populate Open Questions with any genuinely unresolved items surfaced during Steps 3-5
- Use today's date (from Step 2) on all "Asked" timestamps
- If Step 4 question 7 revealed cross-team dependencies → add as Open Questions with owners

```
### Open Questions
1. **Q:** [Question]
   - **Status:** Open
   - **Owner:** [Person]
   - **Priority:** High / Medium / Low
   - **Asked:** YYYY-MM-DD

### Answered Questions
(Questions answered during discovery → move here with their answers)
```

#### Notes & Related Work
- Link to every related JIRA ticket found in Step 3 or mentioned in Step 4
- Explicitly list epics that are candidates to be children of this initiative
- Reference any architecture docs, design docs, or prior explorations mentioned
- Format:

```
## Notes

**Source RFEs** _(the RFEs this initiative implements)_:
* [RHAIRFE-XXX](https://issues.redhat.com/browse/RHAIRFE-XXX) "{RFE Title}" — Status: {status} | "{1-sentence summary of what the RFE requests}"
* [RHAIRFE-XXX](https://issues.redhat.com/browse/RHAIRFE-XXX) "{RFE Title}" — Status: {status} | "{1-sentence summary}"
[If no source RFEs: omit this section entirely]

**Related RFEs** _(related but not fully in scope for this initiative)_:
* [RHAIRFE-XXX](https://issues.redhat.com/browse/RHAIRFE-XXX) "{RFE Title}" — adjacent, tracked for Phase 2
[If none: omit this section]

**Related Initiatives:**
* [{JN-XXX}](https://YOUR_ORG.atlassian.net/browse/JN-XXX) "{Title}" — [relationship: depends on / supersedes / adjacent to]

**Candidate Child Epics (no parent yet):**
* [{JN-XXX}](https://YOUR_ORG.atlassian.net/browse/JN-XXX) "{Epic Title}" — should be linked under this initiative

**Prior Work / Context:**
* [Description of prior attempt, design doc, or constraint]

**Architecture Constraints:**
* [Relevant decisions already made that this initiative must respect]
```

---

## Step 7 — Save the Artifact

Save the complete draft to:
```
artifacts/initiatives/initiative-{slug}-{date}.md
```

Where:
- `{slug}` = 3-5 word kebab-case summary of the title (e.g., `unified-benchmark-logging`)
- `{date}` = today's date in `YYYY-MM-DD` format

Prepend the file with this YAML frontmatter:
```yaml
---
status: draft
jira_key: ""
created: YYYY-MM-DD
title: "[full initiative title here]"
assignee: ""
priority: Medium
source_rfes: []           # list of RHAIRFE keys this initiative implements
related_rfes: []          # list of RHAIRFE keys that are related but out of scope
related_initiatives: []   # list of JN keys found in discovery
candidate_epics: []       # list of JN keys that should be children
---
```

---

## Step 8 — Output to User

After saving, tell the user:

1. The artifact file path
2. A brief summary: title + 2-sentence description of what the initiative covers
3. A **Related Work Summary** showing what was found and linked:
   ```
   🔗 Related Work Linked
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
   Source RFEs (implements):        {N} — {RHAIRFE-XXX, ...}
   Related RFEs (out of scope):     {N}
   Related initiatives referenced:  {N}
   Candidate child epics:           {N}
   Open questions captured:         {N}
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
   ```
4. Suggest next step: "Run `/initiative.review` to score and auto-improve this draft, or edit the file directly before reviewing."

---

## Quality Checklist (Self-Check Before Saving)

Before writing the file, verify every item:

- [ ] Title starts with an action verb
- [ ] Title is 60-80 characters and specific enough to understand without reading the description
- [ ] Context section describes the CURRENT STATE specifically (not just abstract problem)
- [ ] Goals are measurable (not vague like "improve" or "enhance")
- [ ] Scope has both in-scope AND out-of-scope items (out-of-scope informed by adjacent initiatives)
- [ ] DoD has at least 3 verifiable criteria
- [ ] Q&A section is present with open questions populated from discovery
- [ ] If source RFEs exist: Context has an RFE origin paragraph referencing customer need
- [ ] If source RFEs exist: at least one Goal traces back to RFE acceptance criteria
- [ ] Notes section has a "Source RFEs" block with links (or is absent if no RFEs confirmed)
- [ ] Notes section references all related JIRA tickets found
- [ ] Candidate child epics are listed
- [ ] Frontmatter `source_rfes` array is populated (or empty if none confirmed)
- [ ] All timestamps use the date from `mcp_time_get_current_time`
- [ ] File is saved to the correct path with updated frontmatter

If any item is missing → fix it before saving.

---

## Arguments

`$ARGUMENTS` — Optional. The user's problem statement or initiative description. If not provided, ask the user to describe the initiative they want to create.
