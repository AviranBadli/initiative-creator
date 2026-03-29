# Initiative Create Skill

Generate a complete, structured JIRA Initiative draft from a problem statement. Ask clarifying questions first, then produce a fully-formed artifact file.

---

## Step 1 — Load Context

Before asking any questions, read these files silently to internalize all rules and templates:

1. `.cursor/skills/initiative-creator/initiative-template.md` — canonical section structure
2. `guidelines/initiative-guidelines.md` — writing rules, DoD requirements, best practices
3. `guidelines/issue-creation-guidelines.md` — checklist and quality bar

Do NOT skip this step. These files define what "complete" means for an initiative.

---

## Step 2 — Get Current Date

Call `mcp_time_get_current_time` with timezone `Asia/Jerusalem` immediately. Store the result — you will use it for all timestamps in the Q&A section and the artifact frontmatter.

---

## Step 3 — Clarifying Questions

If the user provided only a vague problem statement or a short description, ask ALL of the following questions in a single message (numbered list, do not ask one at a time):

1. **Problem:** What specific problem or pain point does this initiative solve? Who is currently affected and how?
2. **Goals:** What are the 3-6 key outcomes that define success? Try to make them measurable.
3. **Scope:** What is explicitly IN scope? What are you intentionally leaving OUT for later?
4. **Definition of Done:** How will you know this initiative is 100% complete? What can you verify?
5. **Timeline / Phases:** Is this single-phase or multi-phase? Roughly how many sprints do you expect?
6. **Dependencies:** Are there other teams, external systems, or pending decisions this depends on?

If the user already provided detailed context that answers most of these, skip the questions you can already answer and only ask about genuine gaps. If everything is clear, skip Step 3 entirely and proceed to Step 4.

---

## Step 4 — Draft the Initiative

Using the answers from Step 3 (and the user's original description), produce a complete initiative draft.

### Section Requirements

Follow the structure from `initiative-template.md` exactly. Every section below is **required**:

#### Title
- Format: `[Action Verb] [What] [Purpose/Outcome]`
- 60-80 characters max
- Examples: "Allow users to run LLM benchmarks easily and intuitively", "Build unified logging system for JBenchmark"
- Must NOT be vague (no "Improve X" or "Work on Y")

#### Context (2-4 paragraphs)
- Current state and pain points
- Why this matters now (urgency, strategic alignment)
- Who is affected
- Business or technical drivers

#### Goals (3-6 bullets)
- Specific and measurable
- Outcome-focused, not activity-focused
- Bold key terms for scannability

#### Scope
- Bullet list of what is included
- Sub-section "Out of Scope" with what is explicitly excluded

#### Expected Impact
- Quantifiable where possible
- Connect to business outcomes

#### Phases & Milestones _(only if multi-phase)_
- Include only if the initiative spans multiple distinct phases
- Each phase gets a name, 2-3 sentence description, and deliverables list
- Remove this section entirely for single-phase work

#### Definition of Done
- Verifiable, specific criteria — not "improve" but "reduce by X%" or "all Y are complete"
- 3-6 bullet points minimum
- A reader should be able to check each item off with a yes/no

#### Questions & Answers
- Always include this section even if empty
- Add any genuinely open questions from the conversation (owner = Aviran Badli by default)
- Use today's date (from Step 2) on all "Asked" timestamps
- Structure:

```
### Open Questions
1. **Q:** [Question]
   - **Status:** Open
   - **Owner:** [Person]
   - **Priority:** High / Medium / Low
   - **Asked:** YYYY-MM-DD

### Answered Questions
(None yet)
```

#### Notes
- Include links to related Google Docs, JIRA tickets, Slack threads if mentioned
- Note any relevant cross-team dependencies

---

## Step 5 — Save the Artifact

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
assignee: "712020:f6dc72f9-3c15-4b11-82f2-298a7f78d125"
priority: Medium
---
```

---

## Step 6 — Output to User

After saving, tell the user:

1. The artifact file path
2. A brief summary of what was generated (title + 2-sentence description of what the initiative covers)
3. Suggest the next step: "Run `/initiative.review` to score and auto-improve this draft, or edit the file directly before reviewing."

---

## Quality Checklist (Self-Check Before Saving)

Before writing the file, verify every item:

- [ ] Title starts with an action verb
- [ ] Title is 60-80 characters and specific enough to understand without reading the description
- [ ] Context section has at least 2 full paragraphs explaining WHY
- [ ] Goals are measurable (not vague like "improve" or "enhance")
- [ ] Scope has both in-scope AND out-of-scope items
- [ ] DoD has at least 3 verifiable criteria
- [ ] Q&A section is present with correct date format
- [ ] All timestamps use the date from `mcp_time_get_current_time`
- [ ] File is saved to the correct path

If any item is missing → fix it before saving.

---

## Arguments

`$ARGUMENTS` — Optional. The user's problem statement or initiative description. If not provided, ask the user to describe the initiative they want to create.
