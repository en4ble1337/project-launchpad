# PRD Generator

Create detailed Product Requirements Documents that are clear, actionable, and suitable for implementation by junior developers or AI coding agents.

---

## The Job

1. Receive a feature or product description from the user
2. Conduct a structured discovery interview (minimum 2 rounds of questions)
3. Generate a comprehensive PRD based on answers
4. Save to `docs/PRD.md`

**Important:** Do NOT start implementing. Just create the PRD.

---

## Step 1: Discovery Interview

This is the most critical step. A weak interview produces a weak PRD. You are acting as a senior Product Manager conducting discovery, not a passive order taker.

### Round 1: Core Questions (Mandatory)

Always ask **6 to 10 questions** across the categories below, even when the initial prompt is detailed. Your job is to surface hidden assumptions, not just fill obvious gaps. A detailed prompt does not mean a complete one. Every description has implicit decisions baked in that need to be made explicit.

**Format questions with lettered options** so the user can respond quickly (e.g., "1A, 2C, 3B"). Always include an "Other" option for open-ended responses.

**Required question categories (ask at least one from each):**

**Problem and Goal**
Why does this need to exist? What pain point or opportunity does it address? What happens if we do not build it?

**Target Users**
Who exactly is this for? What is their technical skill level? Are there multiple user types with different needs and expectations? What does the primary persona care about most?

**Core Functionality**
What are the 3 to 5 most important actions a user should be able to take? Walk me through the ideal first-time user experience from start to finish.

**User Experience and Flow**
How many steps or clicks should the core action take? What should the first 30 seconds feel like for a new user? What happens on error states? What does an empty state look like?

**Scope and Boundaries**
What should this explicitly NOT do? Are there features you are intentionally deferring to a later version? What is the absolute minimum feature set you would ship?

**Environment and Technical Context**
Where does this deploy? Are there existing systems this must integrate with? Any hard constraints on tech stack, hosting, or third-party services? What is the auth story?

**Success Criteria**
How do we know this is done? What measurable outcome would make this a success? What is the bar for "good enough" versus "polished"?

**Prioritization**
If you could only ship half of what you described, which half? What is non-negotiable versus nice-to-have?

### Example Round 1 Questions:

```
1. What is the primary problem this solves?
   A. Users need a centralized place to share multiple links
   B. Existing solutions are too expensive for basic features
   C. Users want full ownership of their data
   D. Other: [please specify]

2. Who is the primary user?
   A. Non-technical creators (social media influencers, freelancers)
   B. Developers who want a self-hosted alternative
   C. Small businesses managing their online presence
   D. Mix of technical and non-technical users

3. What is their technical comfort level?
   A. Can fill out forms and pick options, zero coding ability
   B. Comfortable with basic web tools, some HTML knowledge
   C. Developer-level, will customize and self-host
   D. Varies significantly by user segment

4. What does the ideal first-time experience look like?
   A. Sign up and have a published page in under 2 minutes
   B. Guided walkthrough with templates and examples
   C. Quick setup with deep customization available later
   D. Other: [please specify]

5. What is the deployment target?
   A. Managed cloud (Vercel, Netlify, etc.)
   B. Self-hosted on user's own infrastructure
   C. Docker container for easy deployment anywhere
   D. Other: [please specify]

6. What is your auth requirement?
   A. Email/password only
   B. Social login (Google, GitHub, etc.)
   C. Both email/password and social login
   D. No auth needed (single user)

7. What is the absolute minimum you would ship?
   A. Just the profile page with links (no themes, no analytics)
   B. Profile page with themes but no analytics
   C. Full featured with analytics from day one
   D. Other: [please specify]

8. Are there any existing systems this needs to integrate with?
   A. No, this is a standalone product
   B. Yes, needs to connect to existing auth/database
   C. Yes, needs API integration with third-party services
   D. Other: [please specify]
```

This lets users respond with "1A, 2B, 3A, 4A, 5A, 6C, 7B, 8A" for quick iteration.

**Wait for their response before proceeding.**

### Round 2: Follow-Up Questions (Mandatory)

After receiving Round 1 answers, identify 3 to 5 follow-up questions that dig deeper into the **most complex, risky, or ambiguous areas** revealed by their answers. This round targets:

**Edge Cases:** What happens when things go wrong? Concurrent users? Data limits? Invalid input? Duplicate entries?

**Unresolved Trade-offs:** Where their answers revealed tension between competing goals (e.g., "simple for creators" versus "customizable for developers"), surface that trade-off explicitly.

**Missing Details on Complex Features:** For any feature that involves multiple screens, state management, or real-time behavior, ask specifically about the expected interaction patterns.

**Example Round 2 Follow-ups:**

```
Based on your answers, a few more questions:

1. You mentioned both non-technical creators and developers as users.
   When these needs conflict, who wins?
   A. Always prioritize simplicity for creators
   B. Provide a simple default with a power-user mode
   C. Lean toward the developer audience
   D. Other: [please specify]

2. For the live preview in the editor, what should happen with unsaved changes?
   A. Warn before navigating away, explicit save button
   B. Auto-save every few seconds
   C. Auto-save on each individual change
   D. Other: [please specify]

3. If a user tries to claim a username/slug that is taken, what happens?
   A. Show "not available" and suggest alternatives
   B. Just show "not available," user picks their own
   C. Allow waitlisting for taken slugs
   D. Other: [please specify]
```

**Wait for their response before generating the PRD.**

---

## Step 2: PRD Structure

Generate the PRD with **all** of the following sections. Sections marked **(Optional)** can be omitted only if truly irrelevant to the project.

### 1. Executive Summary
A concise paragraph (3 to 5 sentences) that tells anyone, whether a stakeholder, a developer, or an AI agent, exactly what this product is and why it exists. This is the elevator pitch. Anyone should understand the product after reading this paragraph alone.

### 2. Mission and Core Principles
A one-sentence mission statement followed by 3 to 5 guiding principles that act as decision filters. When the implementer faces an ambiguous choice, these principles should resolve it.

**Format:**
```markdown
**Mission Statement:** [One sentence describing the product's reason for existing]

**Core Principles:**
1. **[Principle Name]** — [What it means in practice]
2. **[Principle Name]** — [What it means in practice]
3. **[Principle Name]** — [What it means in practice]
```

### 3. Target Users
Define each user persona with:
- **Who they are** (role, context)
- **Technical comfort level** (low, medium, high)
- **Key needs** (what they care about most)

If there are multiple personas, identify the primary one and note where secondary personas' needs may conflict.

### 4. Scope
Two explicit subsections:

**In Scope:** A checklist of everything this version will include. Features, technical requirements, and deployment targets.

**Out of Scope:** Everything this version will NOT include. This is just as important as what is in scope. Be specific. "No admin panel" is better than "no advanced features."

### 5. User Stories
Each story needs:
- **Title:** Short descriptive name
- **Description:** "As a [user], I want [feature] so that [benefit]"
- **Example:** A concrete narrative showing exactly how the interaction plays out with realistic data
- **Acceptance Criteria:** Verifiable checklist of what "done" means

Each story should be small enough to implement in one focused session.

**Format:**
```markdown
### US-001: [Title]
**Description:** As a [user], I want [feature] so that [benefit].

**Example:** [Concrete scenario with realistic data showing the interaction step by step]

**Acceptance Criteria:**
- [ ] Specific verifiable criterion
- [ ] Another criterion
- [ ] Typecheck/lint passes
- [ ] **[UI stories only]** Verify in browser using dev-browser skill
```

**Rules:**
* Acceptance criteria must be verifiable, not vague. "Works correctly" is bad. "Button shows confirmation dialog before deleting" is good.
* **For any story with UI changes:** Always include "Verify in browser using dev-browser skill" as acceptance criteria. This ensures visual verification of frontend work.
* Include edge case behavior in acceptance criteria where relevant (empty states, error states, validation failures).

### 6. Feature Specifications
For each major feature area, provide a detailed breakdown that ties related user stories together into a coherent feature description. This is the "picture on the LEGO box" that shows how individual stories combine into a complete experience.

Include:
- **Route or location** in the app
- **UI behavior** (layout, responsiveness, interaction patterns)
- **State management** notes (what is client-side, what persists)
- **Edge cases** and error handling specific to this feature

User stories tell you WHAT to build one piece at a time. Feature specifications tell you HOW those pieces fit together and what the finished feature should look and feel like.

### 7. Functional Requirements
Numbered list of specific functionalities:

* "FR-1: The system must allow users to..."
* "FR-2: When a user clicks X, the system must..."

Be explicit and unambiguous. Each requirement should be testable in isolation.

### 8. Non-Goals (Out of Scope Detail)
Expand on the Scope section with reasoning. Why are specific features deferred? This prevents the implementer from "helpfully" adding features that were intentionally excluded.

### 9. Design Considerations (Optional)
* UI/UX requirements
* Layout behavior across breakpoints
* Link to mockups if available
* Relevant existing components to reuse
* Accessibility requirements

### 10. Technical Considerations (Optional)
* Known constraints or dependencies
* Integration points with existing systems
* Performance requirements
* Environment variables and configuration

### 11. Implementation Phases
For projects with more than one logical chunk of work, define sequential phases with:
- **Phase goal** (one sentence)
- **Deliverables** (checklist)
- **Validation criteria** (how do we know this phase is complete?)

Phases should be ordered so that each phase builds on the previous one. No phase should depend on work that has not been completed in an earlier phase.

For simple, single-feature PRDs, this section can be omitted.

### 12. Risks and Mitigations
Identify 3 to 5 things that could go wrong and how to handle them.

| Risk | Impact | Mitigation |
|------|--------|------------|
| [What could go wrong] | [How bad is it: High/Medium/Low] | [What to do about it] |

Risks are not the same as open questions. Questions are things you do not know yet. Risks are things that could go wrong even when you know the answer.

### 13. Future Considerations
Features and enhancements intentionally deferred to post-MVP. Listing them here serves two purposes:
1. It tells the implementer "yes, we thought about this, and no, not now"
2. It ensures current architecture decisions do not accidentally block future work

### 14. Success Metrics
How will success be measured?

* "Reduce time to complete X by 50%"
* "Increase conversion rate by 10%"
* "User can go from signup to published page in under 2 minutes"

### 15. Open Questions
Remaining questions or areas needing clarification before or during implementation.

---

## Writing for Junior Developers

The PRD reader may be a junior developer or AI agent. Therefore:

* Be explicit and unambiguous
* Avoid jargon or explain it when first used
* Provide enough detail to understand purpose and core logic
* Number requirements for easy reference
* Use concrete examples with realistic data where helpful
* When describing UI behavior, specify breakpoints and responsive rules
* When describing data, give sample values not just types

---

## Output

* **Format:** Markdown (`.md`)
* **Location:** `docs/`
* **Filename:** `PRD.md` (kebab-case)

---

## Example PRD

```markdown
# PRD: Task Priority System

## Executive Summary

Add priority levels to the existing task management app so users can mark tasks as high, medium, or low priority. This gives users visual cues and filtering tools to focus on what matters most, reducing the "everything looks equally important" problem that leads to missed deadlines.

## Mission and Core Principles

**Mission Statement:** Help users focus on what matters most by making task importance visible and actionable.

**Core Principles:**
1. **Zero friction** — Changing a priority should take one click, not a modal or multi-step flow.
2. **Visible at a glance** — Priority must be apparent without hovering, clicking, or expanding anything.
3. **Non-disruptive** — Adding priority should not change existing workflows or break current task sorting.

## Target Users

**Primary: Task List Power Users**
- People managing 20+ tasks at a time
- Medium technical comfort (use web apps daily, not developers)
- Key need: quickly identify what to work on next without scanning every task

## Scope

**In Scope:**
- Priority field on tasks (high/medium/low)
- Visual priority badges on task cards
- Priority selector in task edit view
- Filter tasks by priority level
- Default new tasks to medium priority

**Out of Scope:**
- Priority-based notifications or reminders
- Automatic priority assignment based on due date
- Priority inheritance for subtasks
- Keyboard shortcuts for priority changes

## User Stories

### US-001: Add priority field to database
**Description:** As a developer, I need to store task priority so it persists across sessions.

**Example:** A task currently stored as `{ id: 1, title: "Write docs", status: "in-progress" }` should become `{ id: 1, title: "Write docs", status: "in-progress", priority: "medium" }`.

**Acceptance Criteria:**
- [ ] Add priority column to tasks table: 'high' | 'medium' | 'low' (default 'medium')
- [ ] Existing tasks receive 'medium' priority via migration
- [ ] Generate and run migration successfully
- [ ] Typecheck passes

### US-002: Display priority indicator on task cards
**Description:** As a user, I want to see task priority at a glance so I know what needs attention first.

**Example:** Looking at my task list, I see a red badge labeled "High" next to "Fix production bug," a yellow badge next to "Update README," and a gray badge next to "Clean up old branches." Without clicking anything, I immediately know which task to tackle first.

**Acceptance Criteria:**
- [ ] Each task card shows colored priority badge (red=high, yellow=medium, gray=low)
- [ ] Badge displays the priority label text ("High", "Medium", "Low")
- [ ] Priority visible without hovering or clicking
- [ ] Badge does not push task title to a second line on mobile (320px width)
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

### US-003: Add priority selector to task edit
**Description:** As a user, I want to change a task's priority when editing it.

**Example:** I open the edit view for "Write docs." I see a dropdown currently showing "Medium." I click it, select "High," and the dropdown immediately shows "High." I click Save, and back on the task list the badge is now red.

**Acceptance Criteria:**
- [ ] Priority dropdown in task edit modal with three options
- [ ] Shows current priority as selected on open
- [ ] Selection updates local state immediately (preview shows new badge)
- [ ] Change persists only after clicking Save
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

### US-004: Filter tasks by priority
**Description:** As a user, I want to filter the task list to see only high-priority items when I'm focused.

**Example:** My task list shows 25 tasks. I select "High" from the filter dropdown. The list now shows only 6 tasks, all with red badges. The URL updates to `?priority=high`. I refresh the page and the filter is still applied. I select "All" to see everything again.

**Acceptance Criteria:**
- [ ] Filter dropdown with options: All | High | Medium | Low
- [ ] Filter persists in URL params (`?priority=high`)
- [ ] Page refresh preserves active filter
- [ ] Empty state message "No tasks match this filter" when no results
- [ ] Task count updates to reflect filtered results
- [ ] Typecheck passes
- [ ] Verify in browser using dev-browser skill

## Feature Specifications

### Priority on Task Cards
**Location:** Main task list view

The priority badge appears to the left of the task title on each card. On desktop (1024px+), badges and titles sit on the same line. On mobile (<1024px), the badge sits above the title to preserve readability. Badges use the existing badge component with new color variants (red, yellow, gray). The badge never truncates or wraps.

### Priority Filter
**Location:** Task list header, right-aligned next to existing sort controls

A single dropdown select element. Default state shows "All." Selecting a priority value filters the visible list client-side and updates the URL search param. The filter works in combination with existing status filters (if both are active, tasks must match both).

## Functional Requirements

- FR-1: Add `priority` field to tasks table ('high' | 'medium' | 'low', default 'medium')
- FR-2: Migrate existing tasks to 'medium' priority
- FR-3: Display colored priority badge on each task card
- FR-4: Include priority selector dropdown in task edit view
- FR-5: Add priority filter dropdown to task list header
- FR-6: Persist filter selection in URL search params
- FR-7: Show empty state message when filter returns zero results

## Non-Goals (Out of Scope Detail)

- **No priority-based notifications:** We considered sending reminders for overdue high-priority tasks but this requires a notification system that does not exist yet. Defer to a future phase.
- **No automatic priority assignment:** Setting priority based on due date proximity was discussed but adds complexity and removes user control. Users should set priority intentionally.
- **No priority inheritance for subtasks:** Subtask priority should be independent. A high-priority epic may have low-priority cleanup subtasks.
- **No keyboard shortcuts:** Worth adding later but not needed for MVP. Current click-based interaction is sufficient.

## Risks and Mitigations

| Risk | Impact | Mitigation |
|------|--------|------------|
| Adding a column to a large tasks table could lock the table during migration | Medium | Run migration during low-traffic hours. Use a non-blocking ALTER TABLE if the database supports it. |
| Priority filter combined with status filter may create confusing empty states | Low | Show a clear message indicating both active filters and a "Clear all filters" button. |
| Users may expect priority to affect sort order automatically | Medium | Document that priority badges are visual only. Add sort-by-priority as a fast follow if usage data supports it. |

## Future Considerations

- Sort by priority within each status column (high first, then medium, then low)
- Keyboard shortcuts for quick priority changes (e.g., Alt+1/2/3)
- Bulk priority updates for multiple selected tasks
- Color-blind accessible badge variants (patterns in addition to colors)

## Success Metrics

- Users can change priority in under 2 clicks from the task list
- High-priority tasks are immediately visually distinguishable at 320px viewport width
- No regression in task list load time (< 200ms added latency)
- Filter usage tracked via analytics (target: 30% of active users use filter within first month)

## Open Questions

- Should the default sort order within a status column factor in priority? (Deferred to future consideration for now)
- Should we track priority change history for audit purposes?
```

---

## Checklist

Before saving the PRD:

* [ ] Conducted Round 1 discovery with at least 6 questions across multiple categories
* [ ] Conducted Round 2 follow-up with at least 3 targeted questions
* [ ] Incorporated all user answers into the document
* [ ] Executive summary is clear enough for a stranger to understand the product
* [ ] Core principles can resolve ambiguous implementation decisions
* [ ] Target users are defined with skill levels and key needs
* [ ] Both In Scope and Out of Scope lists are present and specific
* [ ] User stories are small, specific, and include concrete examples
* [ ] Feature specifications tie related stories together
* [ ] Functional requirements are numbered and unambiguous
* [ ] Non-goals include reasoning for why each was deferred
* [ ] Risks are identified with mitigations (not just open questions)
* [ ] Future considerations prevent scope creep
* [ ] Saved to `docs/PRD.md`
