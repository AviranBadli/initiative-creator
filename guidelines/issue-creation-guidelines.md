# JIRA Issue Creation Guidelines

**Document Owner:** YOUR_NAME  
**Team:** YOUR_TEAM_NAME  
**Last Updated:** December 2025

---

## Core Principles (Apply to ALL Issue Types)

### 1. Clarity Above All

Every issue must be **self-explanatory**. A reader should understand the full scope without asking questions.

**Test yourself:** Can someone who reads this issue for the first time understand:
- ✅ What problem are we solving?
- ✅ What needs to be done?
- ✅ How do we know it's complete?
- ✅ What is NOT included?

If any answer is "No" → Rewrite until clear.

---

### 2. Title Rules (MANDATORY)

**Every title MUST:**
- ✅ Start with an **action verb** (Build, Create, Implement, Enable, Add, Fix, Migrate, etc.)
- ✅ Summarize the **entire scope** in one line
- ✅ Be specific enough that someone can understand the work without reading the description
- ✅ Be 60-80 characters max

**Title Formula:**
```
[Action Verb] + [What] + [Purpose/Outcome]
```

**Good Examples:**

| Type | Title |
|------|-------|
| Initiative | Enable complete infrastructure visibility through GitOps |
| Initiative | Allow users to run LLM benchmarks easily and intuitively |
| Epic | Build cost tracking dashboard for benchmark runs |
| Epic | Implement real-time benchmark monitoring UI |
| Story | Add filter by GPU type to benchmark history page |
| Story | Create API endpoint for triggering new benchmarks |

**Bad Examples:**

| Title | Problem |
|-------|---------|
| "Infrastructure work" | No verb, too vague |
| "UI improvements" | No verb, not specific |
| "Benchmarks" | No verb, no context |
| "Dashboard" | No verb, no action |
| "Fix bug" | Too vague, which bug? |

---

### 3. Q&A Section (REQUIRED for Initiative & Epic)

Every Initiative and Epic **MUST** include a Q&A section to track decisions and clarifications.

**Format:**
```markdown
## Questions & Answers

### Open Questions
1. **Q:** [Question]
   - **Owner:** [Who should answer]
   - **Priority:** High/Medium/Low
   - **Asked:** YYYY-MM-DD

### Answered Questions
1. **Q:** [Question]
   - **A:** [Answer]
   - **Answered by:** [Person]
   - **Date:** YYYY-MM-DD
```

---

## Initiative Guidelines

### Purpose

An Initiative represents a **large body of work** spanning multiple sprints, typically broken into several Epics.

### Structure

```markdown
# [Action Verb] + [What] + [Purpose]

[1-2 sentence summary]

## Context

[2-4 paragraphs explaining:]
- Current state and pain points
- Why this matters now
- Who is affected
- Business/technical drivers

## Goals

* [Goal 1 - specific and measurable]
* [Goal 2 - specific and measurable]
* [Goal 3 - specific and measurable]

## Scope

* [Deliverable 1]
* [Deliverable 2]
* [Deliverable 3]

### Out of Scope
* [What is NOT included]
* [Future work]

## Expected Impact

* [Impact 1 with metrics if possible]
* [Impact 2 with metrics if possible]

## Definition of Done

* [Specific, verifiable criterion 1]
* [Specific, verifiable criterion 2]
* [Specific, verifiable criterion 3]

## Questions & Answers

### Open Questions
[List any open questions]

### Answered Questions
[Track decisions made]

## Notes

* [Related documents]
* [Important context]
```

### Initiative Checklist

Before creating an Initiative, verify:

- [ ] Title starts with action verb
- [ ] Title summarizes the entire initiative scope
- [ ] Context explains WHY (not just WHAT)
- [ ] Goals are specific and measurable
- [ ] Scope clearly defines what's IN and OUT
- [ ] DoD has verifiable completion criteria
- [ ] Q&A section is included
- [ ] A reader can understand everything without asking questions

---

## Epic Guidelines

### Purpose

An Epic represents a **significant feature or capability** that can be completed in 1-3 sprints, broken into multiple Stories.

### Hierarchy

```
Initiative
  └── Epic (you are here)
        └── Story
              └── Sub-task (optional)
```

### Structure

```markdown
# [Action Verb] + [Feature/Capability] + [Outcome]

[1-2 sentence summary of what this epic delivers]

## Context

[1-2 paragraphs explaining:]
- How this epic contributes to the parent initiative
- What user need or problem it addresses
- Key dependencies or constraints

## Goals

* [Goal 1]
* [Goal 2]
* [Goal 3]

## Scope

* [Deliverable 1]
* [Deliverable 2]

### Out of Scope
* [What is NOT included in this epic]

## Definition of Done

* [Specific criterion 1]
* [Specific criterion 2]
* [Specific criterion 3]

## Questions & Answers

### Open Questions
[List any open questions]

### Answered Questions
[Track decisions made]

## Notes

* Parent Initiative: [JN-XXXX]
* Related Epics: [if any]
* Key stakeholders: [names]
```

### Epic Checklist

Before creating an Epic, verify:

- [ ] Title starts with action verb
- [ ] Title clearly describes the feature/capability being built
- [ ] Linked to parent Initiative
- [ ] Context explains how this epic fits the bigger picture
- [ ] Scope is achievable in 1-3 sprints
- [ ] DoD is clear and verifiable
- [ ] Q&A section is included
- [ ] Can be broken into 3-10 Stories

---

## Story Guidelines

### Purpose

A Story represents a **single unit of work** that can be completed by one person in one sprint (ideally 1-3 days).

### Hierarchy

```
Initiative
  └── Epic
        └── Story (you are here)
              └── Sub-task (optional)
```

### Structure

```markdown
# [Action Verb] + [Specific Task] + [Context]

## Description

[1-3 sentences explaining what needs to be done]

## Acceptance Criteria

- [ ] [Criterion 1 - specific and testable]
- [ ] [Criterion 2 - specific and testable]
- [ ] [Criterion 3 - specific and testable]

## Notes

* Parent Epic: [JN-XXXX]
* [Any additional context]
```

### Story Title Examples

| Good ✅ | Bad ❌ |
|---------|--------|
| Add filter dropdown for GPU type selection | GPU filter |
| Create REST endpoint to trigger benchmark | API work |
| Display cost breakdown chart by cloud provider | Show costs |
| Implement SSO login with Red Hat credentials | Login |
| Fix null pointer exception in results parser | Fix bug |

### Acceptance Criteria Rules

Each criterion must be:
- **Specific** - No ambiguity
- **Testable** - Can verify pass/fail
- **Independent** - Can be checked separately

**Good Criteria:**
```
- [ ] User can select GPU type from dropdown (H100, A100, L4)
- [ ] Selected filter persists after page refresh
- [ ] Results table updates within 2 seconds of filter change
```

**Bad Criteria:**
```
- [ ] Filter works (too vague)
- [ ] Good performance (not measurable)
- [ ] User is happy (not testable)
```

### Story Checklist

Before creating a Story, verify:

- [ ] Title starts with action verb
- [ ] Title is specific enough to understand the task
- [ ] Linked to parent Epic
- [ ] Description is clear (1-3 sentences)
- [ ] Acceptance Criteria are specific and testable
- [ ] Story can be completed in 1-3 days
- [ ] A developer can start working without asking questions

---

## Quick Reference: Action Verbs

| Category | Verbs |
|----------|-------|
| **Create/Build** | Build, Create, Develop, Implement, Design, Establish |
| **Modify** | Add, Update, Modify, Enhance, Extend, Improve, Refactor |
| **Enable** | Enable, Allow, Support, Provide, Expose |
| **Fix** | Fix, Resolve, Correct, Handle, Address |
| **Remove** | Remove, Delete, Deprecate, Clean, Archive |
| **Migrate** | Migrate, Move, Transfer, Convert, Upgrade |
| **Integrate** | Integrate, Connect, Link, Sync, Align |
| **Document** | Document, Write, Publish, Define, Specify |
| **Validate** | Validate, Verify, Test, Ensure, Confirm |

---

## Summary Table

| Aspect | Initiative | Epic | Story |
|--------|------------|------|-------|
| **Scope** | Large (multi-sprint) | Medium (1-3 sprints) | Small (1-3 days) |
| **Title** | Action verb + full scope | Action verb + feature | Action verb + specific task |
| **Context** | 2-4 paragraphs | 1-2 paragraphs | 1-3 sentences |
| **Goals** | Required (3-6) | Required (2-4) | Not needed |
| **Scope section** | Required (In + Out) | Required (In + Out) | Not needed |
| **DoD** | Required (detailed) | Required | Via Acceptance Criteria |
| **Q&A Section** | Required | Required | Optional |
| **Acceptance Criteria** | Not needed | Not needed | Required |
| **Contains** | Epics | Stories | Sub-tasks (optional) |

---

## Final Clarity Check

Before submitting ANY issue, ask yourself:

1. **Does the title tell the whole story?**
   - Can someone understand what this is about from the title alone?

2. **Is everything explained?**
   - Would a new team member understand this without asking questions?

3. **Are the completion criteria clear?**
   - Will there be any debate about whether this is "done"?

4. **Is there a Q&A section?** (Initiative/Epic)
   - Are open questions documented?
   - Are past decisions recorded?

**If you answer "No" to any question → Rewrite before creating.**

---

## Examples from Our Team

### Initiative Example

**Title:** Allow users to run LLM benchmarks easily and intuitively

**Link:** [JN-3097](https://jounce.atlassian.net/browse/JN-3097)

---

### Epic Example

**Title:** Build cost tracking dashboard for benchmark runs

**Description:**
```
Create a dashboard that displays cost breakdown by cloud, GPU type, 
user, and time period for all benchmark runs.

## Context
Currently we have no visibility into benchmark costs. Users run 
benchmarks without knowing how much they cost, leading to budget 
overruns and inefficient resource usage.

## Goals
* Provide real-time cost visibility per benchmark run
* Enable cost breakdown by multiple dimensions
* Identify cost outliers and trends

## Scope
* Cost per benchmark run display
* Breakdown by cloud (GCP, AWS, IBM)
* Breakdown by GPU type
* Breakdown by user
* Time-based trend charts

### Out of Scope
* Budget alerts (Phase 2)
* Cost predictions (Phase 2)

## Definition of Done
* Dashboard displays cost for all completed benchmarks
* Users can filter by cloud, GPU, user, and date range
* Cost data updates within 5 minutes of benchmark completion
* Documentation is complete

## Questions & Answers

### Open Questions
1. **Q:** Should we show estimated cost for running benchmarks?
   - **Owner:** YOUR_NAME
   - **Priority:** Medium
   - **Asked:** 2025-12-16

### Answered Questions
(None yet)
```

---

### Story Example

**Title:** Add filter dropdown for GPU type selection

**Description:**
```
Add a dropdown filter to the benchmark history page that allows 
users to filter results by GPU type.

## Acceptance Criteria

- [ ] Dropdown displays all GPU types: H100, A100-80, A100-40, L4
- [ ] Selecting a GPU type filters the table immediately
- [ ] "All" option shows all results
- [ ] Selected filter persists after page refresh
- [ ] Filter works in combination with other existing filters

## Notes
* Parent Epic: JN-XXXX (Cost tracking dashboard)
* Design: Follow existing filter dropdown pattern
```

---

## Contact

Questions about these guidelines? Contact:
- **YOUR_NAME** - YOUR_EMAIL@company.com
- **Team Slack:** #your-team-channel
