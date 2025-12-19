# Project Brief & North Star

<!--
PCE → Traditional PM Mapping:
This file = Your product vision doc + PRD "Background" section
Purpose: Give AI the "why" behind your product so it makes aligned decisions
Update frequency: Rarely (only when vision pivots or major strategy changes)
-->

## 1. Vision
* **One-Liner**: [What is the project in one sentence?]

<!-- EXAMPLE:
TaskFlow is a project management tool that adapts to your workflow instead of forcing you into rigid processes.
-->

* **Elevator Pitch**: [The 30-second pitch describing the problem and solution.]

<!-- EXAMPLE:
Most project management tools force teams into Scrum or Kanban. TaskFlow learns your team's natural workflow and suggests improvements without disrupting how you already work. It's the PM tool that adapts to you, not the other way around.
-->

* **North Star Metric**: [The single metric that best captures the core value delivered to your customers.]

<!-- EXAMPLE:
Weekly active teams using custom workflows (not default templates)

Why this metric?
- Proves product delivers on core promise (workflow adaptation)
- Excludes trial users just trying defaults
- Captures sustained engagement (weekly, not monthly)
-->

---

## 2. Problem Statement
* **The Pain**: [What problem are we solving?]

<!-- EXAMPLE:
Product teams spend 10+ hours per month adapting to their PM tool instead of shipping product. Tools like Jira and Asana force rigid workflows that don't match how creative teams actually work.
-->

* **Current Alternatives**: [How do users solve this today? Why is that inadequate?]

<!-- EXAMPLE:
Current alternatives:
1. Jira/Asana - Rigid workflows, steep learning curve, over-engineered for small teams
2. Notion - Flexible but requires building everything from scratch, no built-in PM features
3. Spreadsheets - Maximum flexibility, zero structure, falls apart at scale

Why inadequate?
- Rigid tools kill creativity with process overhead
- Flexible tools require too much setup
- No tool learns from your team's behavior
-->

---

## 3. The Solution
* **Core Value Proposition**: [Why us?]

<!-- EXAMPLE:
TaskFlow watches how your team works and automatically suggests workflow improvements. It's the only PM tool that learns your process instead of forcing a new one.

Unique moat: ML-driven workflow analysis (competitors offer templates, we offer adaptation)
-->

* **Key Features (High Level)**:

<!-- EXAMPLE:
* Workflow Learning Engine - Analyzes your team's patterns and suggests optimizations
* Flexible Task Views - Kanban, list, calendar, timeline - switch without friction
* Smart Notifications - Only pings when it matters (based on your behavior, not defaults)
* Git Integration - Links PRs to tasks automatically
-->

---

## 4. User Personas
* **Primary Persona**: [Who is the main user?]

<!-- EXAMPLE:
Product Manager at a startup (5-20 person team)

Demographics:
- 28-45 years old
- 3-10 years PM experience
- Previously used Jira/Asana and hated them
- Technical enough to use Git/CLI but prefers UI

Goal: Ship features fast without drowning in process overhead

Frustration: Current tools force them to be "process police" instead of product strategists
-->

    * *Goal*: [What do they want to achieve?]

<!-- EXAMPLE:
Keep team aligned without micromanaging. See what everyone's working on at a glance. Spend time on strategy, not updating task statuses.
-->

    * *Frustration*: [What blocks them?]

<!-- EXAMPLE:
- Jira is overkill (built for enterprise, not startups)
- Asana is too rigid (forces workflows that don't fit)
- Notion is too blank (requires building everything)
- Developers hate updating tasks (so PM tool is always stale)
-->

---

## 5. Success Criteria
* [ ] **MVP Goal**: ...

<!-- EXAMPLE:
[ ] MVP Goal: 50 teams using TaskFlow for >30 days without reverting to old tool
[ ] Key validation: Teams report spending <2 hours/week on process (vs 10+ before)
-->

* [ ] **V1 Goal**: ...

<!-- EXAMPLE:
[ ] V1 Goal: 500 teams, $50k MRR, <5% churn
[ ] Workflow Learning Engine suggests 3+ useful optimizations per team per month
[ ] 80% of teams use custom workflows (not default templates)
-->

---

## Notes for AI

When building features for this product:
- Prioritize **simplicity** over power-user features
- Default to **smart automation** over manual configuration
- If a feature requires >2 clicks, rethink the UX
- Never force a workflow - always suggest, never mandate
- Target is startup PMs, not enterprise project managers

**Out of Scope** (Don't build these without explicit PRD):
- Time tracking (not our focus, integrations exist)
- Resource planning / capacity management (too enterprise)
- Billing/invoicing (not a PM tool feature)
- Advanced reporting beyond basic dashboards
