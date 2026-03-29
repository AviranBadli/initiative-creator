# Initiative Feasibility Skill

Run a structured adversarial review of an initiative from three independent angles — technical feasibility, scope clarity, and timeline/dependency realism — then synthesize findings into an actionable report.

Inspired by rfe-creator's multi-reviewer pattern (feasibility-review, scope-review, architecture-review).

---

## Input

`$ARGUMENTS` — One of:
- A JIRA initiative key (e.g., `JN-3097`)
- A file path to a local initiative artifact
- Nothing — auto-detect the most recently modified initiative in `artifacts/initiatives/`

---

## Step 1 — Load Initiative

Read `skills/initiative-template.md`, `guidelines/initiative-guidelines.md`, `guidelines/issue-creation-guidelines.md`.

Load the initiative content (from JIRA or file). Call `mcp_time_get_current_time`.

If JIRA key provided, also fetch related epics and any linked issues for additional context.

---

## Step 2 — Run Three Independent Reviews

Run all three reviews simultaneously (parallel reasoning). Each reviewer has a distinct focus and adversarial mindset — they are looking for problems, not trying to validate the work.

---

### Reviewer 1: Technical Feasibility

**Focus:** Can this actually be built with the team's current stack, skills, and constraints?

Evaluate:
- Are the technical components mentioned realistic? (APIs, integrations, data models, infrastructure)
- Does the initiative assume capabilities or systems that don't exist yet?
- Are there hidden technical dependencies not called out in the initiative?
- Does the team have the skills to execute this, or does it require new expertise?
- Are there known architectural constraints that make parts of this harder than stated?
- Is the DoD technically verifiable, or are some criteria subjective/unmeasurable?

**Output format:**
```
## Technical Feasibility Review

**Verdict:** PASS / CAUTION / FAIL

**Risks identified:**
1. [Risk] — [Specific concern and why it's a problem]
2. [Risk] — [Specific concern and why it's a problem]

**Assumptions that need validation:**
1. [Assumption] — [Why this needs to be confirmed]

**Recommended additions to DoD:**
- [Criterion that would make completion more verifiable]

**Overall confidence:** High / Medium / Low
**Reasoning:** [2-3 sentence summary]
```

---

### Reviewer 2: Scope & Boundary Clarity

**Focus:** Is the scope precise enough to prevent drift and conflict with adjacent work?

Evaluate:
- Is the Out of Scope section specific enough? Or does it leave ambiguous grey areas?
- Are there items in scope that overlap with known existing initiatives or epics?
- Could two engineers disagree about whether a given task is in or out of scope?
- Is the initiative trying to solve multiple independent problems (should it be split)?
- Are the goals measurable? Could someone claim the initiative is done when it isn't?
- Does the initiative have a clear owner, or is ownership ambiguous?

**Output format:**
```
## Scope & Boundary Review

**Verdict:** PASS / CAUTION / FAIL

**Ambiguities found:**
1. [Ambiguity] — [What's unclear and what could go wrong]
2. [Ambiguity] — [What's unclear and what could go wrong]

**Potential overlaps:**
- [Item that might conflict with existing work]

**Scope creep risks:**
- [Area where scope could silently expand]

**Split recommendation:** YES — consider splitting into [A] and [B] / NO — scope is appropriate

**Overall confidence:** High / Medium / Low
**Reasoning:** [2-3 sentence summary]
```

---

### Reviewer 3: Timeline & Dependency Realism

**Focus:** Is the expected timeline realistic given the team's velocity and known dependencies?

Evaluate:
- Are all external dependencies named and owned? (other teams, third-party APIs, decisions pending)
- Are there circular dependencies or blockers that could stall the initiative before it starts?
- Is the phase breakdown realistic? Are phases properly sequenced?
- Does the initiative depend on work that hasn't been scoped or scheduled yet?
- Are there seasonal constraints (sprints, planning cycles, release windows) not accounted for?
- Is the DoD achievable within the implied timeline?

**Output format:**
```
## Timeline & Dependency Review

**Verdict:** PASS / CAUTION / FAIL

**Blockers identified:**
1. [Blocker] — [Who owns it, what happens if it's delayed]
2. [Blocker] — [Who owns it, what happens if it's delayed]

**Unowned dependencies:**
- [Dependency without a clear owner or ETA]

**Phase sequencing issues:**
- [Issue with ordering or timing]

**Recommended Q&A additions:**
1. Q: [Question that must be answered before starting]
   Owner: [Who should answer]

**Overall confidence:** High / Medium / Low
**Reasoning:** [2-3 sentence summary]
```

---

## Step 3 — Synthesize into Final Report

After all three reviews, produce a consolidated report:

```
📊 Initiative Feasibility Report
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Initiative: "{title}" ({JN-key or file})
Date: {today}

OVERALL VERDICT: ✅ READY / ⚠️ NEEDS WORK / ❌ NOT READY

┌─────────────────────────┬──────────┐
│ Technical Feasibility   │ {PASS/CAUTION/FAIL} │
│ Scope & Boundary        │ {PASS/CAUTION/FAIL} │
│ Timeline & Dependencies │ {PASS/CAUTION/FAIL} │
└─────────────────────────┴──────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CRITICAL ISSUES (must fix before submitting):

1. [Issue from any reviewer] — [What needs to change]
2. [Issue from any reviewer] — [What needs to change]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RECOMMENDED IMPROVEMENTS:

1. [Improvement] — [Why it helps]
2. [Improvement] — [Why it helps]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
OPEN QUESTIONS TO ADD:

1. Q: [Question]  Owner: [Person]  Priority: High/Medium
2. Q: [Question]  Owner: [Person]  Priority: High/Medium

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Step 4 — Offer Auto-Fix

After presenting the report, ask:

```
Should I apply the recommended fixes to the initiative draft automatically?
(yes — apply all / pick specific ones / no — I'll edit manually)
```

If yes:
- Add recommended Q&A entries to the Open Questions section
- Strengthen vague Out of Scope items
- Add any missing DoD criteria
- Save the updated file to `artifacts/initiatives/` with `status: reviewed`

If the initiative was loaded from JIRA (not a local file), save a new local file:
```
artifacts/initiatives/initiative-{JN-key}-feasibility-{date}.md
```

---

## Step 5 — Output

```
✅ Feasibility Review Complete

Overall verdict: {READY / NEEDS WORK / NOT READY}
Critical issues: {N}
Recommended improvements: {N}
Open questions added: {N}

{If fixes applied:}
Updated file: artifacts/initiatives/initiative-{slug}-{date}.md
Next: Run /initiative.review for quality scoring, then /initiative.submit
```
