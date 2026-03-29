
# Initiative Creation Guidelines

## When to Use This Guideline

Use this guideline when creating new JIRA initiatives (issue type: Initiative) for the Model Validation and Benchmarking team in Jounce JIRA.

---

## ⚠️ CRITICAL: Before Working on ANY Initiative

**READ THE INITIATIVE MULTIPLE TIMES** before starting any work:

1. **First read:** Get the general context and understand the problem space
2. **Second read:** Focus on the Goals section - what are we trying to achieve?
3. **Third read:** Study the DoD (Definition of Done) - what does "complete" mean?
4. **Fourth read:** Review scope boundaries and phases - what's in and out of scope?

**DO NOT START WORK until you can answer:**
- ✅ What problem are we solving and why?
- ✅ What specific outcomes define success?
- ✅ How will we know when this initiative is complete?
- ✅ What are we explicitly NOT doing in this initiative?

**If anything is unclear after multiple readings, add questions to the Q&A section and get clarification BEFORE proceeding.**

---

## Initiative Structure Template

### 1. **Title/Summary**
- **Format:** Clear, action-oriented description (60-80 characters max)
- **Pattern:** `[Action Verb] [What] [Purpose/Outcome]`
- **Examples:**
  - ✅ "Allow users to run LLM benchmarks easily and intuitively"
  - ✅ "Build 'Can You Run It – JBenchmark Edition' Interactive UI"
  - ✅ "Establish JBenchmark as an official Red Hat open-source project"
  - ❌ "Benchmarking improvements" (too vague)
  - ❌ "Work on UI" (not descriptive enough)

---

### 2. **Description Section**

#### **A. Context/Problem Statement** (Required)
Start with clear context explaining WHY this initiative exists.

**Format:**
```markdown
[Opening statement summarizing the initiative]

## **Context**

[2-4 paragraphs explaining:]
- Current state and pain points
- Why this matters now
- Who is affected by the problem
- Business or technical drivers
```

**Example:**
```markdown
Create a unified, customer-facing logging system for JBenchmark that provides 
clear, structured, searchable, and filterable logs across the entire benchmark lifecycle.

## **Context**

As JBenchmark evolves into a customer-facing product, it must behave as a single 
coherent service. Today logs are fragmented across containers and components, 
making troubleshooting slow and unclear.

We need consolidated, contextualized logging at the workload level, with full 
lifecycle visibility, structured errors, and a central UI for log exploration.
```

---

#### **B. Goals/Objectives** (Required)
Clear, measurable outcomes the initiative should achieve.

**⚠️ READ CAREFULLY:** Goals define the "what" we're trying to achieve. Before starting any work on an initiative, read the Goals section multiple times until you can explain each goal in your own words. If goals are unclear, add questions to the Q&A section.

**Format:**
```markdown
## **Goals**

* [Goal 1 - specific and measurable]
* [Goal 2 - specific and measurable]
* [Goal 3 - specific and measurable]
```

**Best Practices:**
- Use bullet points for scannability
- Make goals specific and measurable when possible
- Focus on outcomes, not activities
- 3-6 goals is ideal (not too few, not overwhelming)

**Example:**
```markdown
## **Goals**

* Provide a **single point of truth** per benchmark workload
* Enable **full lifecycle tracking** with structured states and timestamps
* Standardize error classification: infra / runtime / auth / quotas / internal bugs
* Ensure logs are **searchable, filterable, and queryable** by internal teams and customers
* Expose logs through a user-friendly interface (Kibana / OpenSearch / etc)
```

---

#### **C. Scope** (Required)
What is included in this initiative and what is explicitly out of scope.

**Format:**
```markdown
## **Scope**

* [Specific deliverable or work area 1]
* [Specific deliverable or work area 2]
* [Specific deliverable or work area 3]

### **Out of Scope** (Optional but recommended)
* [What is explicitly NOT included]
* [Future work that will come later]
```

**Best Practices:**
- Be specific about what's included
- Call out dependencies or integrations
- If scope is large, consider subsections
- Explicitly state what's out of scope to prevent scope creep

**Example:**
```markdown
## **Scope**

* Prepare core **JBenchmark** components (runner, schema, sample workloads) for open-source release
* Define the **public repository structure** under Red Hat GitHub
* Remove or isolate internal-only dependencies (e.g., RHOAI integrations, internal credentials)
* Clean the codebase: remove comments, internal notes, and references
* Add and finalize documentation (README, CONTRIBUTING, LICENSE, SECURITY)
* Work with Legal, Branding, and OSPO to validate repository setup and licensing

### **Out of Scope**
* Migration of existing internal benchmarks to public repo
* Customer onboarding and training materials (Phase 2)
```

---

#### **D. Expected Impact/Outcomes** (Optional but Recommended)
Quantifiable or qualitative impact of completing this initiative.

**Format:**
```markdown
## **Expected Impact**

* [Impact area 1 with metrics if possible]
* [Impact area 2 with metrics if possible]
* [Impact area 3 with metrics if possible]
```

**Example:**
```markdown
## **Expected Impact**

* Reduced cost and redundant deployments
* Improved time-to-validation for benchmarks and tests
* Early GPUaaS prototype adopted across at least 3 teams
* Shared dashboards and observability metrics in production
* Reduction of duplicated provisioning efforts by >70%
```

---

#### **E. Phases & Milestones** (Use for Multi-Phase Initiatives)
Break down complex initiatives into sequential phases.

**Format:**
```markdown
## **Phases & Milestones**

### **Phase 1: [Phase Name]**
[2-3 sentences describing this phase]

**Deliverables:**
* [Deliverable 1]
* [Deliverable 2]

---

### **Phase 2: [Phase Name]**
[2-3 sentences describing this phase]

**Deliverables:**
* [Deliverable 1]
* [Deliverable 2]
```

**Best Practices:**
- Use phases for initiatives spanning multiple sprints
- Each phase should have clear deliverables
- Phases should build on each other logically
- 3-7 phases is typical for large initiatives

---

#### **F. Definition of Done (DoD)** (Required)
Specific, verifiable acceptance criteria for completing the initiative.

**⚠️ CRITICAL IMPORTANCE:** The DoD is the most important section of any initiative. Contributors MUST read and understand the DoD completely before starting work. This defines success - everything else is context. If you're working on an initiative, you should be able to recite the DoD from memory.

**Format:**
```markdown
## **DoD**

* [Specific, measurable completion criterion 1]
* [Specific, measurable completion criterion 2]
* [Specific, measurable completion criterion 3]

**OR** (for more complex initiatives)

## **DoD**

This work is considered complete only when all the following are delivered:

1. **[Deliverable Category 1]**
   
   [Detailed explanation of what complete means]

2. **[Deliverable Category 2]**
   
   [Detailed explanation of what complete means]
```

**Best Practices:**
- Be specific and measurable
- Use verifiable criteria (not "improve" but "reduce by X%")
- Include review/approval steps when relevant
- Tie to business outcomes when possible

**Example:**
```markdown
## **DoD**

* All selected models are successfully validated and benchmarked
* Evaluation and performance data has passed integrity, correctness, and validation checks
* All data has been successfully transferred to the Model Catalog ingestion pipeline
* The Model Catalog display has been inspected and confirmed as accurate and complete
* Product manager and team leader have approved the final display
```

---

#### **G. Questions & Answers (Q&A)** (NEW - Recommended)
Track open questions, decisions, and answers throughout the initiative lifecycle.

**Format:**
```markdown
## **Questions & Answers**

### **Open Questions**
1. **Q:** [Question about scope, approach, or dependencies]
   - **Status:** Open
   - **Owner:** [Person responsible for answering]
   - **Priority:** High/Medium/Low
   - **Asked:** YYYY-MM-DD

2. **Q:** [Another open question]
   - **Status:** Open
   - **Owner:** [Person]
   - **Priority:** High/Medium/Low
   - **Asked:** YYYY-MM-DD

---

### **Answered Questions**
1. **Q:** [Previously asked question]
   - **A:** [Answer/decision made]
   - **Answered by:** [Person]
   - **Date:** YYYY-MM-DD
   - **Impact:** [How this affects the initiative]

2. **Q:** [Another answered question]
   - **A:** [Answer]
   - **Answered by:** [Person]
   - **Date:** YYYY-MM-DD
```

**Best Practices:**
- Update Q&A section actively throughout initiative lifecycle
- Move questions from Open to Answered as decisions are made
- Include who owns answering each question
- Document impact of answers on initiative direction
- Use this to track alignment meetings and decisions

**Example:**
```markdown
## **Questions & Answers**

### **Open Questions**
1. **Q:** Should we use OpenSearch or Splunk for log aggregation backend?
   - **Status:** Open
   - **Owner:** YOUR_NAME + Infrastructure team
   - **Priority:** High
   - **Asked:** 2025-11-20

2. **Q:** What is the expected log retention period for customer-facing logs?
   - **Status:** Open
   - **Owner:** Product Management
   - **Priority:** Medium
   - **Asked:** 2025-11-21

---

### **Answered Questions**
1. **Q:** Should logging be synchronous or asynchronous to avoid performance impact?
   - **A:** Asynchronous logging with buffering to minimize performance overhead. 
         Benchmark impact testing showed <2% overhead with async approach vs 15% with sync.
   - **Answered by:** Mark + Eli
   - **Date:** 2025-11-18
   - **Impact:** Architecture updated to use async logging with configurable buffer sizes

2. **Q:** Do we need to support log export to external SIEM systems?
   - **A:** Not in v1. Focus on internal observability first. SIEM integration is Phase 2.
   - **Answered by:** YOUR_NAME
   - **Date:** 2025-11-15
   - **Impact:** Removed SIEM integration from DoD; added to Phase 2 backlog
```

---

#### **H. Notes/References** (Optional)
Links to relevant documents, related work, or important context.

**Format:**
```markdown
## **Notes**

* [Relevant context or consideration]
* Related documents:
  * [Document title](link) - brief description
  * [Another doc](link) - brief description
* [Any other important information]
```

**Example:**
```markdown
## **Notes**

* I found a [document](https://docs.google.com/document/d/...) that might be 
  relevant, not sure how applicable it still is, but worth reviewing
* This work may also be related to the [GPU monitoring task](https://docs.google.com/document/d/...). 
  Worth thinking about the connection and alignment opportunities
* We can reach out to @Cara Delia regarding open-sourcing JBenchmark - 
  she is with OSAIPO and can take it to legal if necessary
```

---

## Priority Guidelines

### Priority Levels (Standard JIRA)
1. **Critical** - Business-critical, major blockers, time-sensitive deliverables
2. **High** - Important work, key features, significant improvements
3. **Medium** - Standard features, improvements, technical debt
4. **Low** - Nice-to-have improvements, minor enhancements
5. **Nice to Have** - Optional work, experimental features, team culture initiatives

### How to Choose Priority
- **Critical:** Revenue impact, executive commitments, major customer blockers, compliance/legal requirements
- **High:** Key product features, important technical foundations, significant user pain points
- **Medium:** Standard roadmap items, incremental improvements, technical debt
- **Low:** Quality of life improvements, minor features, polish work
- **Nice to Have:** Learning initiatives, experimental work, team building activities

---

## Initiative Metadata

### Required Fields
- **Summary/Title:** Clear, descriptive title
- **Description:** Following the structure above (Context, Goals, Scope, DoD)
- **Priority:** One of the five levels above
- **Assignee:** Primary owner (usually manager/tech lead for initiatives)
- **Sprint:** Assign to appropriate sprint (or leave in backlog for future planning)

### Optional But Recommended Fields
- **Labels:** Add relevant labels for filtering (e.g., "customer-facing", "open-source", "infrastructure", "observability")
- **Components:** If your JIRA project uses components
- **Epic Link:** If this initiative relates to a larger body of work

---

## Best Practices

### For Initiative Readers (Engineers/Contributors)
- **⚠️ READ MULTIPLE TIMES:** Never start work after reading an initiative just once. Read it 3-4 times focusing on different aspects each time (context, goals, DoD, scope)
- **Understand the "why":** Make sure you deeply understand the problem being solved before diving into solutions
- **Memorize the DoD:** The Definition of Done is your north star - know it by heart before starting any work
- **Question everything unclear:** If anything is ambiguous after multiple readings, add questions to the Q&A section immediately
- **Confirm understanding:** Discuss the initiative with your team lead to confirm you understand goals and DoD correctly

### Writing Style
- **Be specific, not vague:** "Reduce benchmark runtime by 30%" not "Improve performance"
- **Action-oriented:** Start with verbs and focus on outcomes
- **Scannable:** Use headers, bullets, bold text for key points
- **Balanced detail:** Enough context to understand, not a novel
- **Keep it current:** Update Q&A and status as work progresses

### Content Guidelines
- **Context before details:** Always explain WHY before diving into WHAT
- **User perspective:** Describe impact on users/customers when relevant
- **Link generously:** Reference related docs, epics, meetings, decisions
- **Quantify when possible:** Use metrics, percentages, time estimates
- **Call out risks:** Mention dependencies, blockers, unknowns upfront

### Collaboration
- **Stakeholder alignment:** List key stakeholders and ensure they're aligned
- **Cross-team work:** Explicitly call out dependencies on other teams
- **Review before filing:** Have 1-2 people review the initiative description before creating
- **Living document:** Update the Q&A section actively as the initiative progresses

### Common Pitfalls to Avoid
- ❌ **Too vague:** "Make JBenchmark better"
- ❌ **Too technical:** Diving into implementation before explaining goals
- ❌ **No DoD:** How do we know when this is complete?
- ❌ **Scope creep:** Adding too many goals or unclear boundaries
- ❌ **No ownership:** Who is responsible for answering open questions?
- ❌ **Stale Q&A:** Questions asked but never answered or updated

---

## Example Complete Initiative Template

```markdown
# [Initiative Title]

[1-2 sentence summary of what this initiative achieves]

## **Context**

[2-4 paragraphs explaining the current situation, problem, and why this matters]

## **Goals**

* [Goal 1]
* [Goal 2]
* [Goal 3]

## **Scope**

* [In scope item 1]
* [In scope item 2]

### **Out of Scope**
* [Explicitly not included]

## **Expected Impact**

* [Impact 1 with metrics]
* [Impact 2 with metrics]

## **Phases & Milestones** _(if applicable)_

### **Phase 1: [Name]**
[Description]

**Deliverables:**
* [Deliverable 1]

---

### **Phase 2: [Name]**
[Description]

**Deliverables:**
* [Deliverable 1]

## **Definition of Done**

* [Specific completion criterion 1]
* [Specific completion criterion 2]
* [Specific completion criterion 3]

## **Questions & Answers**

### **Open Questions**
1. **Q:** [Question]
   - **Status:** Open
   - **Owner:** [Person]
   - **Priority:** High/Medium/Low
   - **Asked:** YYYY-MM-DD

---

### **Answered Questions**
1. **Q:** [Question]
   - **A:** [Answer]
   - **Answered by:** [Person]
   - **Date:** YYYY-MM-DD
   - **Impact:** [How this affects direction]

## **Notes**

* [Relevant context]
* Related documents:
  * [Doc title](link) - description
```

---

## Workflow: Creating a New Initiative

1. **Understand the need:** Clarify with stakeholders what problem you're solving
2. **Draft context:** Write the context section first - if you can't explain WHY, stop and clarify
3. **Define goals:** List 3-6 specific goals that define success
4. **Set boundaries:** Be explicit about scope and out-of-scope
5. **Write DoD:** Create verifiable acceptance criteria
6. **Add Q&A section:** List any open questions upfront
7. **Get feedback:** Share draft with 1-2 key stakeholders
8. **Revise and create:** Update based on feedback, then create the JIRA initiative
9. **Update actively:** Keep the Q&A section current as work progresses
10. **Close with learnings:** When complete, add a brief retrospective note

---

## Workflow: Working on an Existing Initiative

**⚠️ BEFORE STARTING ANY WORK:**

1. **Read the initiative 3-4 times** - Focus on different sections each time
2. **Understand Goals deeply** - What outcomes define success?
3. **Memorize the DoD** - How will you know when you're done?
4. **Review Scope boundaries** - What's included and what's explicitly out?
5. **Check Q&A section** - Read all answered questions for context and decisions
6. **Ask questions** - Add any unclear items to Q&A section
7. **Discuss with team lead** - Confirm your understanding before starting
8. **ONLY THEN begin work** - Never start after just one quick read

**Remember:** Initiatives represent significant work spanning multiple sprints. Taking 30 minutes to fully understand the initiative will save weeks of misaligned work.

---

## Agent Instructions

When creating a new initiative using these guidelines:

1. **Always include Q&A section** - Even if starting with no questions, include the section structure
2. **Ask clarifying questions** - If context or goals are unclear, ask the user before drafting
3. **Use examples from this doc** - Reference the patterns and examples provided
4. **Default to comprehensive** - Better to include all sections and remove what's not needed
5. **Link to related work** - Reference existing initiatives, epics, docs
6. **Get the current date** - Use `mcp_time_get_current_time` for Q&A timestamps
7. **Review before submitting** - Check that the initiative follows the structure and best practices

When working on an existing initiative:

1. **READ THE FULL INITIATIVE** - Never work on an initiative after scanning it once
2. **Summarize Goals and DoD** - Before starting any work, summarize what the initiative is trying to achieve and how completion is defined
3. **Confirm understanding** - If working on a complex initiative, confirm with the user that you understand the goals and DoD correctly
4. **Reference DoD constantly** - Throughout the work, continuously check that your actions align with the Definition of Done
5. **Update Q&A** - If you encounter ambiguity, add questions to the Q&A section rather than making assumptions

---

## Related Guidelines

- **Epic Creation:** See epic-specific guidelines for breaking down initiatives
- **Story Creation:** See story guidelines for implementation tasks under initiatives
- **RFE Guidelines:** See `rfe-guidelines.mdc` for customer-facing feature requests
