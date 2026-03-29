# Initiative Review Skill

Score an initiative draft against the quality checklist and auto-revise detected issues. Up to 2 revision cycles before presenting the final output.

---

## Input

`$ARGUMENTS` — One of:
- A file path to a local draft (e.g., `artifacts/initiatives/initiative-unified-logging-2026-03-29.md`)
- A JIRA issue key (e.g., `JN-3097`) — the skill will fetch the initiative from JIRA via Atlassian MCP
- Nothing — the skill will look for the most recently modified file in `artifacts/initiatives/` with `status: draft`

---

## Step 1 — Load the Initiative

**If a file path is provided:** Read the file directly.

**If a JIRA key is provided:** Fetch the issue using the Atlassian MCP:
- Cloud ID: `511393e2-4fb8-40dd-aa81-0b7818a52bfc`
- Use `expand: "changelog,comments"` to get full context
- Extract Summary (title) and Description (body) as the content to review

**If no argument:** Find the most recently modified `.md` file in `artifacts/initiatives/` that has `status: draft` in its frontmatter.

Also read these files silently to load the quality standards:
- `skills/initiative-template.md`
- `guidelines/initiative-guidelines.md`
- `guidelines/issue-creation-guidelines.md`

---

## Step 2 — Score Against the Checklist

Evaluate the initiative against every item in the checklist below. For each item, mark it PASS or FAIL with a brief explanation of why.

### Scoring Checklist

#### Title (max 10 pts)
- [ ] **T1** — Starts with an action verb (Build, Create, Enable, Establish, Implement, etc.)
- [ ] **T2** — Is 60-80 characters and fully descriptive
- [ ] **T3** — Specific enough to understand without reading the description (no "Improve X", "Work on Y")

#### Context (max 15 pts)
- [ ] **C1** — Has at least 2 full paragraphs explaining WHY this initiative exists
- [ ] **C2** — Identifies who is affected (team, customers, external stakeholders)
- [ ] **C3** — Explains why this matters NOW (urgency, timing, strategic driver)

#### Goals (max 15 pts)
- [ ] **G1** — Has 3-6 goals (not too few, not overwhelming)
- [ ] **G2** — Goals are outcome-focused (not activity lists)
- [ ] **G3** — Goals are measurable or specific enough to verify completion

#### Scope (max 15 pts)
- [ ] **S1** — Has explicit in-scope items as a bullet list
- [ ] **S2** — Has an "Out of Scope" sub-section with at least 1 item
- [ ] **S3** — Scope boundaries are clear enough to prevent scope creep

#### Definition of Done (max 20 pts)
- [ ] **D1** — DoD section is present and not empty
- [ ] **D2** — Has at least 3 verifiable criteria
- [ ] **D3** — Criteria are specific — each one can be answered YES or NO
- [ ] **D4** — No vague language ("improve", "better", "good enough")

#### Q&A Section (max 10 pts)
- [ ] **Q1** — Q&A section is present
- [ ] **Q2** — Open Questions and Answered Questions subsections exist
- [ ] **Q3** — Any obvious open questions from the content are captured

#### Expected Impact (max 10 pts)
- [ ] **I1** — Expected Impact section is present
- [ ] **I2** — At least 1 item is quantified or tied to a measurable outcome

#### Overall Quality (max 5 pts)
- [ ] **O1** — A new team member could understand this without asking questions
- [ ] **O2** — No section is copy-paste placeholder text left unfilled

### Score Calculation

Count PASS items. Output a score in this format:

```
Score: XX/100

✅ PASS (N items)
❌ FAIL (N items)
```

---

## Step 3 — Identify Issues

For every FAIL item, write a specific description of the problem and the exact change needed. Group issues by severity:

**Critical (must fix before submit):** T1-T3, D1-D3, S1-S2, C1, Q1-Q2
**Recommended (should fix):** C2-C3, G1-G3, D4, S3, Q3, I1-I2, O1-O2

---

## Step 4 — Auto-Revise (Cycle 1)

Automatically fix ALL Critical issues and ALL Recommended issues without asking for permission. Apply changes directly to the draft content.

After each fix:
- Note what was changed and why (1 line per fix)
- Update the file on disk with the revised content
- Update the frontmatter `status` to `reviewed`

---

## Step 5 — Re-Score (Cycle 2 if needed)

After the first revision, re-run the scoring checklist against the revised draft.

If any Critical items still FAIL:
- Apply a second round of targeted fixes
- Re-score one more time

Maximum 2 revision cycles. After 2 cycles, if Critical items still fail, list them explicitly and ask the user for input — do not attempt a third automatic revision.

---

## Step 6 — Output Review Report

Present the final result to the user in this format:

```markdown
## Initiative Review Report

**File:** artifacts/initiatives/initiative-{slug}-{date}.md
**Status:** reviewed
**Final Score:** XX/100

### Changes Made

1. [Change 1] — why it was needed
2. [Change 2] — why it was needed
...

### Remaining Issues (if any)

[List any items that could not be auto-fixed]

### Next Step

Run `/initiative.submit` to create this initiative in JIRA, or edit the file at [path] and re-run `/initiative.review`.
```

---

## Step 7 — Save Updated File

Write the revised initiative back to disk at the same path it was loaded from. Update frontmatter:
```yaml
status: reviewed
```

If the input was a JIRA key (not a local file), save the revised version as a new local file:
```
artifacts/initiatives/initiative-{jira-key}-reviewed-{date}.md
```
