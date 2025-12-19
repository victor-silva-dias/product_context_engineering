# PRD: [Feature Name]

<!--
PCE → Traditional PM Mapping:
This file = Your standard Product Requirements Document
Purpose: Define WHAT to build and WHY, with clear acceptance criteria
Update frequency: Per feature (create new PRD for each significant feature)
Use: AI reads this to understand feature requirements before coding (Act Mode)

This is the contract between product and engineering. Everything here should be
specific enough for AI to generate correct implementation.
-->

**Status**: [Draft / Approved / In Progress / Shipped]
**Owner**: [Name]
**Created**: [YYYY-MM-DD]
**Last Updated**: [YYYY-MM-DD]

<!-- EXAMPLE:
**Status**: Approved
**Owner**: Victor (PM)
**Created**: 2025-01-10
**Last Updated**: 2025-01-15
-->

## 1. Problem & Goal
* **User Story**: As a [User], I want [Action], so that [Benefit].
* **Business Value**: [Why build this?]

<!-- EXAMPLE:
**User Story**:
As a startup Product Manager using AI coding tools,
I want AI to remember our product vision and constraints between sessions,
So that generated code stays aligned with product strategy (no drift).

**Problem Context**:
- Current pain: AI forgets product decisions between sessions (context amnesia)
- Impact: 40% of AI-generated code requires refactoring to match vision
- User frustration: Repeating product context in every prompt is tedious

**Business Value**:
- Reduces context re-entry time by 80% (30 min → 6 min per session)
- Increases AI code accuracy from 60% → 90% alignment with vision
- Differentiator: No other PM tool solves AI context amnesia
- Supports OKR: "80% of users complete onboarding within 10 minutes"
-->

## 2. Functional Requirements
* **FR-01**: The system must...
* **FR-02**: The system should...

<!-- EXAMPLE:
**FR-01**: System must provide structured templates for product context
- Project vision (project_brief.md)
- Tech stack constraints (tech_stack.md)
- Code patterns (system_patterns.md)

**FR-02**: AI must be able to read these templates from workspace
- Workspace-aware agents (Claude Code, Cursor): Direct file access
- CLI tools (Gemini CLI): Pipe via stdin

**FR-03**: Templates must include inline examples showing how to fill them
- Every section has commented example
- Examples show realistic product scenarios (not generic placeholders)

**FR-04**: Framework must support "Planning Mode" (AI creates PRDs) and "Act Mode" (AI codes)
- Planning Mode: AI reads context + working_docs → generates PRDs/strategy
- Act Mode: AI reads context + PRD → generates code

**FR-05**: All context files must be version-controlled in Git
- Changes tracked over time
- Team can review PRD changes via Pull Requests
-->

## 3. User Experience (UX) Flow
1. User clicks X.
2. System validates Y.
3. System displays Z.

<!-- EXAMPLE:
**Planning Mode Flow** (Creating product vision):
1. User has meeting, captures insights in working_docs/meeting_notes.md
2. User opens AI agent, prompts: "Read product_context_engineering/ and working_docs/, help me refine product vision"
3. AI analyzes existing context + meeting insights
4. AI suggests updates to project_brief.md
5. User reviews, approves, commits changes to Git

**Act Mode Flow** (Coding a feature):
1. User opens AI agent with PRD-001-authentication.md
2. User prompts: "Read product_context_engineering/ and implement login from PRD-001"
3. AI reads project_brief (vision), tech_stack (constraints), system_patterns (code style), PRD-001 (requirements)
4. AI generates login component respecting all constraints
5. User reviews code, tests pass, commits to Git

**Edge Case Flow** (AI violates constraint):
1. AI generates code with TODO comment (violates system_patterns.md)
2. User: "Check system_patterns.md - are TODOs allowed?"
3. AI: "No, system_patterns.md forbids TODO comments. I'll refactor to remove them."
4. AI fixes code
-->

## 4. Technical Specifications
* **API Endpoints**:
    * `POST /api/v1/resource`
* **Data Model Changes**:
    * Add column `is_active` to table `users`.
* **Latency Requirement**: < 200ms.

<!-- EXAMPLE:
**File Structure**:
```
product_context_engineering/
├── 00_project_context/
│   ├── template_01_project_brief.md (with inline examples)
│   ├── template_02_tech_stack.md (with inline examples)
│   └── template_03_system_patterns.md (with inline examples)
├── 01_working_docs/
│   └── template_meeting_notes.md (with inline examples)
├── 02_timeline_and_reports/
│   └── template_sprint_report.md (with inline examples)
├── 03_strategy_and_vision/
│   └── template_strategy_doc.md (with inline examples)
├── 04_product_requirements/
│   └── template_PRD.md (this file, with inline examples)
├── 05_prompts_and_instructions/
│   ├── template_system_prompt.md (with inline examples)
│   └── template_cursor_rules.md (with inline examples)
├── README.md (framework overview)
├── FRAMEWORK_GUIDE.md (Planning/Act Mode workflows)
├── TROUBLESHOOTING.md (common issues)
├── GLOSSARY.md (terminology)
├── LICENSE (MIT)
└── CONTRIBUTING.md (contribution guidelines)
```

**Template Format**:
- Markdown files with HTML comments for examples
- Format: `<!-- EXAMPLE: [realistic filled example] -->`
- PM Translation notes: `<!-- PCE → Traditional PM Mapping: ... -->`

**AI Integration Points**:
- Workspace-aware agents: AI reads workspace files directly
- CLI tools: Pipe context via stdin (e.g., `cat product_context_engineering/*.md | ai-cli`)
- Simple prompt pattern: "Read product_context_engineering/ and [task]"

**Performance**:
- Template files: <500 lines each (stay within AI token limits)
- Context injection: <30 seconds for AI to read all files
-->

## 5. Edge Cases & Constraints
* What happens if the internet cuts out?
* What happens if the input is too long?

<!-- EXAMPLE:
**Edge Cases**:

1. **User doesn't create PRD before coding**:
   - AI should flag: "I don't see a PRD for this feature in 04_product_requirements/. Should we create one first?"
   - Prevents undocumented features

2. **Context files conflict** (e.g., PRD says use React, tech_stack says Vue):
   - AI should flag conflict: "PRD-001 mentions React, but tech_stack.md specifies Vue. Which should I use?"
   - Prevents silent assumption-making

3. **Template too long** (>500 lines):
   - Risk: Exceeds AI token limits
   - Mitigation: Split large PRDs into multiple feature-specific PRDs

4. **Team member doesn't update working_docs**:
   - Risk: Decisions lost
   - Mitigation: Add "Update Context" as acceptance criteria in PRDs

5. **AI ignores system_patterns constraints**:
   - Root cause: Constraints buried in long file
   - Mitigation: Move "Forbidden Behaviors" to top of system_patterns.md

**Constraints**:
- Must work with workspace-aware AI agents (Claude Code, Cursor, similar)
- Must support CLI tools for automation (Gemini CLI, similar)
- Templates must be Git-friendly (markdown, no binary files)
- Must support both Planning Mode (AI creates docs) and Act Mode (AI codes)
-->

## 6. Acceptance Criteria
* [ ] Test Case 1 passed.
* [ ] Test Case 2 passed.

<!-- EXAMPLE:
**Acceptance Criteria** (Definition of Done):

**Templates**:
- [ ] All 9 templates have inline examples (commented HTML)
- [ ] All templates have PM Translation notes at top
- [ ] Examples show realistic scenarios (not generic placeholders)
- [ ] Each template <500 lines (token limit friendly)

**Documentation**:
- [ ] README.md explains framework philosophy and quick start
- [ ] AI_INTEGRATION.md shows Planning Mode + Act Mode workflows
- [ ] TROUBLESHOOTING.md covers 5+ common scenarios
- [ ] GLOSSARY.md defines all framework-specific terms

**AI Integration**:
- [ ] Test: Ask AI agent to "Read product_context_engineering/ and help me refine vision"
      → AI generates coherent vision suggestions
- [ ] Test: Ask AI agent to "Read product_context_engineering/ and implement feature X"
      → AI respects constraints from system_patterns.md
- [ ] Test: CLI tool context injection works (pipe files via stdin)

**Version Control**:
- [ ] All files committed to Git
- [ ] LICENSE file present (MIT)
- [ ] CONTRIBUTING.md present

**Community Readiness**:
- [ ] Someone can clone repo and understand framework in <5 minutes (README test)
- [ ] Someone can fill templates and start using in <30 minutes
- [ ] No broken links in documentation
-->
