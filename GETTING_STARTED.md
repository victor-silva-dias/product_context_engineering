# Getting Started with Product Context Engineering

**Get your first AI-powered feature built with full product context in 15 minutes.**

---

## What You'll Accomplish

By the end of this guide, you'll have:
- ✅ A working PCE folder structure
- ✅ Core context files that AI can read
- ✅ Your first AI session using product context
- ✅ A workflow for keeping context updated

---

## Prerequisites

- **Workspace-aware AI agent** (Claude Code, Gemini CLI, Cursor, or similar)
- Basic familiarity with markdown files
- A product you're building (or want to build)

> **Note**: You need an AI agent that can read files from your project. This makes context injection simple - just reference the files.

---

## Step 1: Create the Folder Structure (2 minutes)

### Option A: Clone This Repository

```bash
# Clone the template
git clone https://github.com/yourusername/product_context_engineering.git

# Copy into your existing project
cp -r product_context_engineering/ /path/to/your/project/
```

### Option B: Manual Setup

Create these folders in your project root:

```bash
mkdir -p product_context_engineering/{00_project_context,01_working_docs,02_timeline_and_reports,03_strategy_and_vision,04_product_requirements,05_prompts_and_instructions}
```

### Verify Structure

```
your-project/
├── product_context_engineering/
│   ├── 00_project_context/
│   ├── 01_working_docs/
│   ├── 02_timeline_and_reports/
│   ├── 03_strategy_and_vision/
│   ├── 04_product_requirements/
│   └── 05_prompts_and_instructions/
├── src/ (your code)
└── ... (other project files)
```

---

## Step 2: Write Your First Context File (5 minutes)

Start with the most important file: **project_brief.md**

Create `00_project_context/project_brief.md`:

```markdown
# Project Brief: [Your Product Name]

## Vision
[One sentence: What are you building and why?]

Example: "A legal AI platform that helps lawyers draft contracts 10x faster using verified templates and intelligent clause suggestions."

## Problem Statement
[What problem does this solve?]

Example:
- Lawyers spend 60% of time on repetitive contract drafting
- Manual clause selection leads to inconsistencies
- Junior lawyers lack access to senior expertise

## Target User
[Who is this for?]

Example:
- Primary: Solo practitioners and small law firms (1-10 lawyers)
- Secondary: In-house legal teams at startups

## Success Criteria
[How do you know if this works?]

Example:
- Reduce contract drafting time from 4 hours to 30 minutes
- 80% of generated contracts require minimal edits
- 100 active users within 3 months

## Core Constraints
[Non-negotiable requirements]

Example:
- Must comply with legal ethics (no unauthorized practice of law)
- Data privacy: No training AI on client documents
- Works offline (lawyers work in courtrooms without WiFi)
```

**Fill this out now** - don't overthink it. You can refine later.

---

## Step 3: Add Tech Stack Context (3 minutes)

Create `00_project_context/tech_stack.md`:

```markdown
# Tech Stack

## Core Technologies
- **Frontend**: [Your choice - e.g., React, Vue, vanilla JS]
- **Backend**: [Your choice - e.g., Node.js, Python, Go]
- **Database**: [Your choice - e.g., PostgreSQL, MongoDB, SQLite]
- **AI/LLM**: [Your choice - e.g., OpenAI API, Claude API, local models]

## Architecture Principles
- [Key decision 1 - e.g., "API-first design"]
- [Key decision 2 - e.g., "Mobile-responsive from day one"]
- [Key decision 3 - e.g., "Serverless deployment"]

## Development Environment
- **IDE**: [Your tool - e.g., VSCode, Cursor, Vim]
- **AI Assistant**: [Your tool - e.g., Claude Code, Copilot]
- **Version Control**: Git + GitHub/GitLab
```

**Fill this out** based on your current or planned stack.

---

## Step 4: Define Code Patterns (2 minutes)

Create `00_project_context/system_patterns.md`:

```markdown
# System Patterns & Conventions

## Code Style
- Use [your language style - e.g., "TypeScript strict mode"]
- Maximum file length: 200 lines (split if longer)
- No TODO comments in production code

## Naming Conventions
- Files: `kebab-case.ts`
- Functions: `camelCase()`
- Components: `PascalCase`
- Constants: `SCREAMING_SNAKE_CASE`

## Forbidden Behaviors
- NEVER use `any` type (if TypeScript)
- NEVER commit commented-out code
- NEVER skip error handling on API calls
- NEVER hardcode credentials or API keys

## Directory Structure
[Describe your folder organization]

Example:
```
src/
├── components/  (UI components)
├── services/    (API calls, business logic)
├── utils/       (Helper functions)
└── types/       (TypeScript definitions)
```
```

**Customize this** - these rules ensure AI generates code that matches your style.

---

## Step 5: Test with Your AI Agent (2 minutes)

Now verify that your agent can read and understand the context.

**Tell your AI agent**:
```
"Read product_context_engineering/00_project_context/project_brief.md and summarize our product vision in 2 sentences."
```

**Expected Result**: AI reads the file and accurately describes your vision.

**Success Check** ✅
- Does the agent understand your product?
- Can it suggest features that align with your vision?
- Does it respect your constraints?

If yes → You're ready to build!

---

## Step 6: Create Your First Feature (Ongoing)

Let's build something using the context.

### A. Create a PRD (2 minutes)

Create `04_product_requirements/PRD-001-[feature-name].md`:

```markdown
# PRD-001: [Feature Name]

## Problem
[What user problem does this solve?]

## User Story
As a [who],
I want [what],
So that [why].

## Requirements
- [ ] Requirement 1
- [ ] Requirement 2
- [ ] Requirement 3

## Acceptance Criteria
- [ ] User can [action 1]
- [ ] System displays [expected behavior]
- [ ] Error handling for [edge case]

## Out of Scope
- [Feature X] - planned for later
- [Feature Y] - not needed for MVP
```

### B. Build with AI (Act Mode)

Now implement the feature with full context.

**Tell your AI agent**:
```
"Read product_context_engineering/00_project_context/ and PRD-001-[feature-name].md.
Let's implement this feature."
```

**What happens**:
- AI reads your vision, tech stack, patterns, and PRD
- AI generates code aligned with your product strategy
- AI follows constraints from system_patterns.md
- You review, refine, commit

**Result**: Code that's technically correct AND strategically aligned.

---

## Next Steps

### 🎯 Daily Workflow

**Morning - Start coding session**:
```
"Read product_context_engineering/00_project_context/ and PRD-001.
Let's continue implementing [feature]."
```

**After meetings - Capture decisions**:
```
"Update product_context_engineering/01_working_docs/meeting_notes.md with:
- [Decision 1]
- [Decision 2]"
```

**End of week - Maintain context**:
1. Review `01_working_docs/` - promote mature decisions to proper folders
2. Delete obsolete notes (keep inbox clean)
3. Update PRDs if requirements changed

**The pattern**: Read context → Build/refine → Commit changes to Git

### 📚 Expand When Ready

**Not required initially** - add these layers as your product grows:

- **Strategy** (`03_strategy_and_vision/`) - roadmap, OKRs, priorities
- **Reports** (`02_timeline_and_reports/`) - sprint reports, retrospectives
- **AI Instructions** (`05_prompts_and_instructions/`) - consistent AI behavior rules

Start minimal. Add structure only when it adds value.

### 🔄 Maintenance

**Weekly**: Review working_docs → promote or delete
**Monthly**: Keep context accurate and under 500 lines per file

Context is living documentation - refactor when stale.

---

## Common Mistakes to Avoid

- ❌ Writing too much detail → Be concise, use bullet points
- ❌ Not telling AI to read files → Always reference context explicitly
- ❌ Letting working_docs pile up → Promote or delete weekly
- ❌ Creating PRDs for trivial fixes → Small bug fixes don't need PRDs
- ❌ Forgetting to commit context → Version context like code in Git

---

## You're Ready!

You now have:
- ✅ PCE folder structure
- ✅ Core context files (project_brief, tech_stack, system_patterns)
- ✅ Tested workflow with your AI agent
- ✅ Understanding of the simple pattern

**Next**: Read [FRAMEWORK_GUIDE.md](./FRAMEWORK_GUIDE.md) to understand Planning Mode vs Act Mode.

---

## Quick Reference

### The Simple Pattern

**1. Point AI to context**
```
"Read product_context_engineering/00_project_context/"
```

**2. State your goal**
- Planning: "help me create PRD for [feature]"
- Act: "let's implement [feature from PRD]"

**3. Work together** → Review → Commit to Git

That's it.

### Essential Files

```
00_project_context/       ← Start here
├── project_brief.md      ← Your product vision
├── tech_stack.md         ← Your technologies
└── system_patterns.md    ← Your code style

01_working_docs/          ← Daily notes
04_product_requirements/  ← Feature PRDs

(Other folders: add when needed)
```

---

**Questions?** Check [TROUBLESHOOTING.md](./TROUBLESHOOTING.md) or [GLOSSARY.md](./GLOSSARY.md).
