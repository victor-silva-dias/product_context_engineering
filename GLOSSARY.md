# Glossary

**Terminology used in the Product Context Engineering framework.**

---

## Core Concepts

### Context-as-Code
Treating business and product context as version-controlled, structured documentation that AI agents consume systematically.

**In practice**: Instead of explaining your product vision to AI in every chat session, you write it once in `project_brief.md`, commit it to Git, and reference that file whenever working with AI.

**Analogy**: Just like you wouldn't write code without version control, you shouldn't make product decisions without versioning the context that informs them.

---

### AI Context Injection
The act of pointing an AI tool to specific PCE files before requesting code generation or product decisions.

**Example**:
```
"Read 00_project_context/project_brief.md for product vision,
then help me implement the authentication feature"
```

**Why it matters**: Without injection, AI operates with zero knowledge of your product. With injection, AI becomes a product-aware assistant.

---

### Product Drift
When AI-generated features diverge from product strategy due to lack of business context.

**Symptoms**:
- Features that "work" but don't align with vision
- Technical debt from pursuing "cool" over "right"
- Scope creep driven by AI suggestions

**Prevention**: Inject `project_brief.md` and `strategy_doc.md` before every feature discussion.

---

### Context Amnesia
AI forgetting previous business decisions between sessions.

**Problem**: Chat-based AI has no memory across sessions. Every conversation starts from zero.

**PCE Solution**: Context lives in Git, not chat history. Point AI to the files, and it "remembers" via documentation.

---

### Vibe Coding
Rapid, exploratory development using AI coding assistants (Claude Code, Copilot, etc.) with minimal upfront planning.

**Pro**: Fast iteration, low friction
**Con**: Without PCE, leads to product drift and technical debt

**PCE's Role**: Adds product rigor to vibe coding without killing velocity.

---

## PCE-Specific Terms

### Static Context (00_project_context)
Foundational truths that rarely change.

**Contents**:
- Product vision and mission
- Tech stack and architecture principles
- Code style and design patterns

**Update Frequency**: Monthly or when major decisions change
**Purpose**: The "north star" that AI should treat as read-only truth

---

### Working Context (01_working_docs)
Day-to-day flux - the "inbox" for discoveries.

**Contents**:
- Meeting notes
- Quick decisions
- Ideas in progress
- Temporary reminders

**Update Frequency**: Daily/weekly
**Workflow**: Capture → Refine → Promote to proper layer (or delete)

**Goal**: Inbox Zero - keep under 10 files

---

### Historical Context (02_timeline_and_reports)
Archive of what was built and why.

**Contents**:
- Sprint reports
- Retrospectives
- Post-mortems
- Release notes

**Update Frequency**: After sprints/milestones
**Purpose**: Onboarding new team members (or new AI tools) and learning from past mistakes

---

### Strategic Context (03_strategy_and_vision)
Current roadmap and priorities.

**Contents**:
- OKRs (Objectives & Key Results)
- Product roadmap
- Target market and positioning
- Pricing strategy

**Update Frequency**: When priorities shift (pivots, new markets, etc.)
**Critical**: Changes here trigger AI assumption updates

---

### Feature Context (04_product_requirements)
Specific feature requirements and specifications.

**Contents**:
- PRDs (Product Requirements Documents)
- User stories
- Acceptance criteria
- Edge cases

**Update Frequency**: Per feature
**Purpose**: Convergence point of strategy + execution - the "contract" for what to build

---

### Behavioral Context (05_prompts_and_instructions)
How AI should behave, what to prioritize, what to avoid.

**Contents**:
- System prompts
- Cursor rules / IDE config
- Forbidden behaviors
- Tone and style guidelines

**Update Frequency**: As needed
**Purpose**: Meta-instructions for consistent AI behavior across tools

---

## Traditional PM Terms (for reference)

These are concepts Product Managers already know, which PCE leverages:

### PRD (Product Requirements Document)
A document describing what a feature should do and why.

**Traditional PM**: Written in Confluence, Google Docs, Notion
**PCE**: Written in markdown at `04_product_requirements/PRD-XXX.md`, versioned in Git

**Key Sections**:
- Problem statement
- User stories
- Functional requirements
- Acceptance criteria
- Edge cases

---

### OKR (Objectives & Key Results)
A goal-setting framework.

**Traditional PM**: Tracked in spreadsheets or OKR tools
**PCE**: Documented in `03_strategy_and_vision/strategy_doc.md`

**Structure**:
- **Objective**: Qualitative goal (e.g., "Become the #1 legal AI platform")
- **Key Results**: Measurable outcomes (e.g., "10k MAU by Q2")

---

### User Story
A format for describing features from the user's perspective.

**Format**:
```
As a [who],
I want [what],
So that [why]
```

**Example**:
```
As a product manager,
I want to track API usage over time,
So that I can identify cost spikes before they become problems
```

**In PCE**: Used in `04_product_requirements/` PRDs

---

### Acceptance Criteria
Conditions for considering a feature "done."

**Format**: Checklist of requirements

**Example**:
```
- [ ] User can log in with email and password
- [ ] Invalid credentials show clear error message
- [ ] Session persists for 7 days
- [ ] User can log out
```

**In PCE**: Part of every PRD in `04_product_requirements/`

---

### North Star Metric
The single metric that best captures core value delivered to customers.

**Examples**:
- SaaS: Monthly Active Users (MAU)
- E-commerce: Revenue per user
- Social: Daily engagement time

**In PCE**: Defined in `03_strategy_and_vision/strategy_doc.md`

---

## AI/LLM-Specific Terms

### LLM (Large Language Model)
The AI models powering tools like ChatGPT, Claude, Gemini.

**In PCE context**: The "engine" that reads your context files and generates code/suggestions

---

### Token
Unit of text that LLMs process. Roughly ¾ of a word.

**Why it matters**: LLMs have token limits (e.g., 200k tokens). Large context files can exceed limits.

**PCE Strategy**: Use layered context injection - don't dump all folders into every prompt.

---

### Prompt Engineering
The practice of crafting inputs to LLMs to get desired outputs.

**Traditional approach**: Trial-and-error to find the "magic prompt"
**PCE approach**: Systematic context injection makes prompts repeatable and predictable

---

### Grounding
Providing LLMs with specific source material to reference (vs generating from general knowledge).

**Example**: "Use this PRD [paste PRD] as grounding for implementation"

**PCE**: All context files serve as grounding material for AI

---

## Workflow Terms

### Context Injection
See "AI Context Injection" in Core Concepts above.

---

### Context Promotion
Moving content from `01_working_docs/` to a permanent home.

**Example**:
- Decision made in meeting → Captured in `meeting_notes.md`
- Decision becomes final → Promoted to `04_product_requirements/PRD-XXX.md`
- Original note → Deleted from `working_docs/`

**Goal**: Keep working_docs clean ("inbox zero")

---

### Layered Context
Injecting context progressively based on task complexity.

**Layer 1 (Vision)**: `project_brief.md`
**Layer 2 (Strategy)**: `strategy_doc.md`
**Layer 3 (Requirements)**: `PRD-XXX.md`
**Layer 4 (Constraints)**: `system_patterns.md`

**Use case**: Don't inject all layers for simple questions - be selective.

---

### Forbidden Behaviors
Explicit constraints documented in `system_prompt.md` that AI must never violate.

**Examples**:
- NEVER use `any` type in TypeScript
- NEVER commit TODO comments
- NEVER create files longer than 200 lines

**Purpose**: Set hard boundaries for AI behavior

---

### Inbox Zero (for Working Docs)
Keeping `01_working_docs/` under 10 files by regularly promoting or deleting content.

**Borrowed from**: Email productivity methodology
**Applied to PCE**: Treat working_docs like an inbox - don't let it pile up

---

## Common Abbreviations

| Abbreviation | Full Term | Meaning |
|--------------|-----------|---------|
| **PCE** | Product Context Engineering | This framework |
| **PRD** | Product Requirements Document | Feature specification |
| **OKR** | Objectives & Key Results | Goal-setting framework |
| **LLM** | Large Language Model | AI like GPT, Claude, Gemini |
| **CLI** | Command Line Interface | Terminal-based tools |
| **IDE** | Integrated Development Environment | VSCode, JetBrains, etc. |
| **MAU** | Monthly Active Users | User engagement metric |
| **MVP** | Minimum Viable Product | Simplest version of feature |

---

## Anti-Patterns (What NOT to Do)

### Context Bloat
Writing excessively detailed context files that exceed token limits and confuse AI.

**Solution**: Be concise. Use bullet points, not essays.

---

### Template Rigidity
Following templates religiously even when they add no value.

**Solution**: Progressive adoption - start minimal, add structure as needed.

---

### Chat History Dependence
Relying on AI's conversational memory instead of documented context.

**Problem**: AI forgets between sessions
**Solution**: If it's not in a file, it doesn't exist

---

### Stale Context
Context files that are months out of date and no longer reflect reality.

**Solution**: Treat context like code - refactor when it no longer serves.

---

### Premature Documentation
Writing detailed PRDs for features that may never be built.

**Solution**: Document decisions, not hypotheticals.

---

## Related Concepts

### Shape Up
Basecamp's product development methodology.

**Relation to PCE**: Shape Up "pitches" can become PCE PRDs. Shape Up "appetite" becomes acceptance criteria.

---

### Scrum / Agile
Iterative development frameworks with sprints.

**Relation to PCE**: PCE adds AI context layer. Sprint planning updates `strategy_doc.md`, retros update `sprint_report.md`.

---

### Docs-as-Code
Treating documentation with same rigor as code (version control, CI/CD, reviews).

**Relation to PCE**: PCE is "Context-as-Code" - a specific application of docs-as-code for AI-driven development.

---

## Need More Context?

- **Framework Overview**: See [README](./README.md)
- **How to Use**: See [FRAMEWORK_GUIDE](./FRAMEWORK_GUIDE.md)
- **Common Issues**: See [TROUBLESHOOTING](./TROUBLESHOOTING.md)
