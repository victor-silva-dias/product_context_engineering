# Troubleshooting Guide

**Common issues when using Product Context Engineering and how to solve them.**

---

## "AI Keeps Ignoring My Constraints"

### Symptoms
- AI generates code that violates `system_patterns.md`
- UX requirements from PRDs are ignored
- Architecture principles aren't followed
- Code doesn't match the style guide

### Root Causes
1. **Constraints too vague** - "Write clean code" means nothing to an LLM
2. **Context not injected** - AI never actually read the constraint files
3. **Constraints buried** - Important rules hidden in walls of text
4. **No validation** - AI wasn't asked to confirm understanding

### Solutions

#### ✅ Make Constraints Measurable
**Bad**:
```markdown
- Write clean code
- Follow best practices
- Keep it simple
```

**Good**:
```markdown
- Maximum 200 lines per file
- Maximum 3 parameters per function
- No nested ternary operators
- Never use TODO/FIXME comments in production code
```

#### ✅ Explicit Pre-Prompt
Before AI generates code, ask it to list constraints:

```
"Before implementing, please:
1. Read system_patterns.md
2. List the 5 most important constraints for this feature
3. Confirm you understand them"
```

If AI can't list them correctly, it didn't read or understand the file.

#### ✅ Add to Forbidden Behaviors
In `05_prompts_and_instructions/system_prompt.md`, create a "NEVER DO THIS" section:

```markdown
## Forbidden Behaviors
- NEVER use `any` type in TypeScript
- NEVER commit commented-out code
- NEVER skip error handling on API calls
- NEVER create files longer than 200 lines
```

#### ✅ Test with Simple Constraint First
Start with one obvious constraint and verify AI follows it:

```
"Write a function that calculates totals.
Constraint: Maximum 50 lines.
If you need more, split into helper functions."
```

If AI violates this simple rule, your context injection isn't working.

---

## "Context Files Are Getting Too Large"

### Symptoms
- Hitting token limits when injecting context
- AI gets confused by too much information
- Takes forever to copy-paste context into web chats
- Context feels overwhelming to maintain

### Root Causes
1. **Trying to inject everything** - Dumping all 6 folders into every prompt
2. **PRDs too detailed** - Writing novels instead of specs
3. **No archiving** - Old sprint reports piling up
4. **Duplication** - Same info in multiple places

### Solutions

#### ✅ Use Layered Context Injection
Don't inject all folders. Be selective based on task:

**For Vision Questions**:
```
Read: 00_project_context/project_brief.md only
```

**For Implementation**:
```
Read:
- 00_project_context/system_patterns.md (constraints)
- 04_product_requirements/PRD-XXX.md (requirements)
```

**For Strategic Planning**:
```
Read:
- 00_project_context/project_brief.md (vision)
- 03_strategy_and_vision/strategy_doc.md (roadmap)
```

#### ✅ Split Large PRDs
If a PRD is >500 lines, it's probably multiple features:

**Instead of**:
```
PRD-001-authentication.md (800 lines covering login, signup, password reset, 2FA)
```

**Do**:
```
PRD-001-login.md
PRD-002-signup.md
PRD-003-password-reset.md
PRD-004-two-factor-auth.md
```

#### ✅ Archive Old Content
Create an `/archive` subfolder in `02_timeline_and_reports`:

```
02_timeline_and_reports/
├── sprint-report-2025-12.md (recent, keep accessible)
├── sprint-report-2025-11.md (recent, keep accessible)
└── archive/
    ├── sprint-report-2025-10.md (older, archive)
    └── sprint-report-2025-09.md (older, archive)
```

Only inject recent sprint reports.

#### ✅ Prioritize Context Layers
When forced to choose due to token limits:

**Priority Order**:
1. `00_project_context/` (always needed)
2. `04_product_requirements/` (for implementation)
3. `03_strategy_and_vision/` (for planning)
4. `01_working_docs/` (rarely inject, promote instead)
5. `02_timeline_and_reports/` (for retrospectives only)
6. `05_prompts_and_instructions/` (meta, rarely inject)

---

## "Team Not Updating Working Docs"

### Symptoms
- `01_working_docs/` folder is empty or stale
- Decisions made in calls aren't documented
- Context drift between team members
- AI doesn't "know" about recent decisions

### Root Causes
1. **Friction too high** - Takes effort to update docs
2. **No forcing function** - Nothing reminds people to update
3. **Unclear value** - Team doesn't see why it matters
4. **Started too big** - Tried to document everything at once

### Solutions

#### ✅ Add "Update Context" to Acceptance Criteria
In every PRD, add as a requirement:

```markdown
## Acceptance Criteria
- [ ] Feature works as described
- [ ] Tests pass
- [ ] **working_docs/meeting_notes.md updated with key decisions**
```

Now it's part of "done", not optional.

#### ✅ Weekly Context Review
Like a standup, but for context:

```
Every Friday (10 min):
- Review working_docs from the week
- Decide what to promote (to PRD or system_patterns)
- Decide what to delete (no longer relevant)
- Clear the inbox
```

This creates a rhythm of maintenance.

#### ✅ Make AI Remind the Team
In `05_prompts_and_instructions/system_prompt.md`:

```markdown
After every feature discussion, remind the user:
"Should I update working_docs/meeting_notes.md with this decision?"
```

Now AI becomes the enforcer.

#### ✅ Start Small - Just Meeting Notes
Don't try to document everything. Start with one habit:

**Week 1-2**: After meetings, write 3 bullet points in `meeting_notes.md`
**Week 3-4**: Promote mature decisions to PRDs
**Week 5+**: Expand to other working docs

Build the muscle memory first.

---

## "Not Sure When to Create PRD vs Just Code"

### Symptoms
- Hesitation before starting work
- Some features have PRDs, others don't (inconsistent)
- Unclear what deserves documentation

### Decision Rule

#### ✅ Quick Fix (<30min effort) → Just Code
**Examples**:
- Fix typo in UI text
- Update dependency version
- Fix obvious bug (null check missing)

**Workflow**:
1. Fix it
2. Commit
3. Optionally note in `working_docs/` if it's worth remembering

---

#### ✅ New Feature (any size) → Always PRD First
**Examples**:
- Add user authentication
- Create export functionality
- Build new dashboard

**Workflow**:
1. Write `04_product_requirements/PRD-XXX.md`
2. Include: problem, user stories, acceptance criteria
3. Get it reviewed (if team)
4. Then code

**Why**: Features accumulate complexity. Without a PRD, you'll forget edge cases.

---

#### ✅ Refactor → Depends on Scope
**Small refactor** (<2 files):
- Rename variable
- Extract helper function
- **Action**: Just do it, note in working_docs

**Medium refactor** (2-5 files):
- Restructure module
- Change data model
- **Action**: Write a mini-PRD (just problem + approach)

**Large refactor** (>5 files):
- Rewrite authentication system
- Change database layer
- **Action**: Full PRD with migration plan

---

## "AI Suggests Features Not in Roadmap"

### Symptoms
- AI proposes features you don't want
- Implementation goes off-track
- Scope creep during development

### Root Cause
AI doesn't understand **strategic priorities**. It optimizes for "technically interesting" not "business critical."

### Solutions

#### ✅ Explicit Session Scoping
Start every session with strategic context:

```
"Our current focus (from strategy_doc.md):
- Q1 2026: User authentication and basic workflows
- NOT doing: Advanced analytics, integrations, mobile app

Now, help me implement login flow."
```

This sets boundaries before AI starts generating ideas.

#### ✅ Add "Out of Scope" to PRDs
In `04_product_requirements/PRD-XXX.md`:

```markdown
## Out of Scope (Explicitly Not Included)
- Social login (OAuth) - planned for Q2
- Multi-factor authentication - planned for Q3
- Single Sign-On (SSO) - not on roadmap
```

Now when AI suggests "Should we add Google login?", you can reference the PRD.

#### ✅ Update Strategy Doc When Priorities Change
If you pivot, update `03_strategy_and_vision/strategy_doc.md` immediately:

```markdown
## Current Focus (Updated Jan 2026)
**Shifted priority**: Mobile app moved to Q1 (was Q3)
**Reason**: 60% of users on mobile, can't wait
```

Then tell AI: "Read updated strategy_doc.md before suggesting features"

---

## "Working Docs Keep Piling Up - When to Promote?"

### Symptoms
- `01_working_docs/` has 20+ files
- Can't find recent decisions
- Unclear what's still relevant

### Decision Matrix

| Content Type | Where to Promote | When |
|--------------|------------------|------|
| **Product decision** (e.g., "We're using JWT not sessions") | `04_product_requirements/PRD-XXX.md` or `00_project_context/tech_stack.md` | When decision is final |
| **Architecture pattern** (e.g., "All API calls use this error handler") | `00_project_context/system_patterns.md` | When pattern is established |
| **Strategic shift** (e.g., "Targeting enterprises, not consumers") | `03_strategy_and_vision/strategy_doc.md` | When strategy changes |
| **Historical record** (e.g., "Why we chose PostgreSQL") | `02_timeline_and_reports/` | When sprint ends |
| **Temporary note** (e.g., "Bug to fix tomorrow") | Delete after action | When no longer relevant |

### Promotion Workflow

**Weekly (5-10 min)**:
1. Open `01_working_docs/`
2. Read each file
3. Ask: "Is this still just a note, or has it become a decision?"
4. If decision → Copy to proper folder, delete from working_docs
5. If still in flux → Keep
6. If no longer relevant → Delete

**Goal**: Keep `01_working_docs/` under 10 files

---

## "Template Feels Too Heavy for Small Projects"

### Symptoms
- Solo founder feeling overwhelmed by structure
- Skipping templates because they feel like overkill

### Solution: Progressive Adoption

#### ✅ Minimum Viable Context (Week 1)
Start with just these 2 files:
1. `00_project_context/project_brief.md` (vision)
2. `00_project_context/system_patterns.md` (constraints)

That's it. Just document **what** you're building and **how** you want to build it.

#### ✅ Add PRDs When You Need Them (Week 2+)
Only create `04_product_requirements/PRD-XXX.md` when:
- Feature is complex enough to forget details
- You want AI to remember requirements

Don't create PRDs for tiny features.

#### ✅ Add Other Layers Organically
- **`01_working_docs/`**: Add when you have meetings or need to capture decisions
- **`02_timeline_and_reports/`**: Add when you want to track progress
- **`03_strategy_and_vision/`**: Add when you have a roadmap
- **`05_prompts_and_instructions/`**: Add when you want consistent AI behavior

**Remember**: PCE is a framework, not a religion. Use what helps, skip what doesn't.

---

## "Not Sure How to Measure if PCE is Working"

### Success Indicators

#### ✅ You're Winning If:
- **AI generates code that respects constraints** (first try, no violations)
- **You can onboard new AI tools quickly** (just point to context files)
- **Features stay aligned with vision** (no surprise drift)
- **You remember why decisions were made** (context in Git history)
- **Team is aligned** (everyone reads same context)

#### ❌ You're Losing If:
- **AI constantly violates constraints** (context injection failing)
- **Features drift from roadmap** (strategy not injected)
- **You re-explain product vision repeatedly** (context not working)
- **Context is stale** (no one updates it)
- **Templates feel like busywork** (wrong level of rigor for project size)

### Adjustment Strategy
If PCE feels like overhead, you're probably:
1. **Using too many templates** → Simplify to 2-3 core files
2. **Writing too much detail** → Be more concise
3. **Not injecting context** → AI isn't reading files
4. **Treating it like documentation** → It's for AI, not humans

---

## Still Stuck?

**Check the core docs**:
- [README](./README.md) - Framework overview
- [Framework Guide](./FRAMEWORK_GUIDE.md) - How to inject context
- [Glossary](./GLOSSARY.md) - Term definitions

**Common root cause**: 95% of issues trace back to context not being injected properly. Re-read [FRAMEWORK_GUIDE.md](./FRAMEWORK_GUIDE.md) and verify AI is actually reading your files.
