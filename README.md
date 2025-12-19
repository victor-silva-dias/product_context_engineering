# Product Context Engineering (PCE)

**Context-as-Code: The systematic approach to building AI-driven products without losing your product vision.**

---

## What is Product Context Engineering?

Product Context Engineering (PCE) is a framework that treats your product's business context—vision, strategy, requirements, constraints—as **version-controlled, structured documentation** that AI agents can systematically consume.

Born from the practice of "vibe coding" (rapid AI-assisted development), PCE solves the core problem that Product Managers face when building with LLMs: **how do you move fast with AI without creating product drift?**

Instead of re-explaining your product vision to AI every session, PCE gives you a structured way to maintain a "single source of truth" that lives in Git alongside your code. Think of it as **documentation that programs AI behavior**.

---

## The Problem

Product Managers and builders using AI coding tools (Claude Code, Copilot, Gemini CLI, ChatGPT, etc.) face four critical issues:

### 1. **Context Amnesia**
AI forgets your product decisions between sessions. You re-explain the vision, constraints, and strategy over and over.

### 2. **Product Drift**
LLMs generate features that are technically correct but strategically misaligned. Fast code doesn't equal right product.

### 3. **Constraint Violations**
AI ignores UX requirements, architecture principles, or brand guidelines because it doesn't "know" they exist.

### 4. **Tool Fragmentation**
Your product context is scattered across chat histories, Google Docs, Linear tickets, and tribal knowledge. When you switch AI tools, you start from zero.

---

## The Solution: Context-as-Code

PCE provides a **6-layer directory structure** where you document:

```
product_context_engineering/
├── 00_project_context/      # Static: Vision, tech stack, architecture (rarely changes)
├── 01_working_docs/          # Dynamic: Meeting notes, daily discoveries (high flux)
├── 02_timeline_and_reports/  # Historical: Sprint reports, retrospectives (archive)
├── 03_strategy_and_vision/   # Strategic: Roadmap, OKRs, positioning (pivots trigger updates)
├── 04_product_requirements/  # Feature: PRDs, specs (convergence of strategy + execution)
└── 05_prompts_and_instructions/ # Behavioral: How AI should act, what to prioritize
```

### How It Works

1. **Document your product** using familiar PM concepts (PRDs, user stories, OKRs)
2. **Inject context** into AI before coding: "Read `00_project_context/project_brief.md` then..."
3. **Version everything** in Git so context evolves with your product
4. **Reuse across tools** - works with any IDE, CLI, or chat interface

---

## Philosophy: Vibe Coding + Product Rigor

> **PCE enables pure vibe coding with product alignment.**
>
> Built for rapid AI-assisted development with agents that have workspace access (Claude Code, Gemini CLI). No complex configuration - just point agents to your context folder and build.

**Core Principles**:
- ✅ **Vibe coding first**: Designed for Claude Code, Gemini CLI, and similar workspace-aware agents
- ✅ **Git-based**: All context versioned and portable (no vendor lock-in)
- ✅ **Zero config**: No `.cursorrules` or system prompts needed - just reference context files
- ✅ **PM-familiar**: Uses concepts you already know (PRDs, OKRs, user stories)
- ✅ **AI-specific**: Solves problems unique to LLM-assisted development

**Recommended Tools**:
- **IDEs**: VSCode (with Antigravity), Cursor
- **Agents**: Claude Code (VSCode extension), Gemini CLI

---

## Quick Start (5 Minutes)

### Step 1: Create Your Context Foundation

Set up the basic folder structure and create three core files in `00_project_context/`:

1. **`project_brief.md`** - Your product vision, problem statement, and success criteria
2. **`tech_stack.md`** - Core technologies, architecture principles
3. **`system_patterns.md`** - Code style, directory conventions, design patterns

These rarely change and form the "brain" of your AI context.

### Step 2: Test with Your AI Tool

Before your next coding session:

```
"Read product_context_engineering/00_project_context/ and help me implement [feature]"
```

The AI now understands your product vision before writing code.

### Step 3: Start Building

Create your first PRD in `04_product_requirements/` and let AI build with full product context.

**→ See [GETTING_STARTED.md](./GETTING_STARTED.md) for detailed walkthrough**

---

## Directory Structure Explained

| Folder | Purpose | Update Frequency | Examples |
|--------|---------|------------------|----------|
| **00_project_context** | Foundational truths about your product | Rarely (monthly) | Vision, tech stack, architecture |
| **01_working_docs** | Inbox for daily/weekly discoveries | Daily | Meeting notes, quick decisions |
| **02_timeline_and_reports** | Historical archive | After sprints | Sprint reports, retrospectives |
| **03_strategy_and_vision** | Current strategic direction | When pivoting | Roadmap, OKRs, positioning |
| **04_product_requirements** | Feature specifications | Per feature | PRDs, user stories, acceptance criteria |
| **05_prompts_and_instructions** | AI behavior configuration | As needed | System prompts, forbidden behaviors |

### Decision Tree: When to Update Which Folder?

- 🆕 "I'm planning a new feature" → **04_product_requirements/**
- 🎯 "Business strategy changed" → **03_strategy_and_vision/**
- 📝 "Quick meeting note" → **01_working_docs/**
- 📊 "Sprint ended" → **02_timeline_and_reports/**
- 🏗️ "Architecture decision" → **00_project_context/system_patterns.md**

---

## Works With Your Existing PM Process

PCE is **complementary**, not a replacement:

- **Using Scrum?** Sprint planning updates `strategy_doc.md`, retros update `sprint_report.md`
- **Using Kanban?** Each card links to a PRD file
- **Using Shape Up?** Shaped pitches become PRDs, appetite becomes acceptance criteria

PCE adds the **AI context layer** to whatever process you already use.

---

## Vibe Coding Examples

### With Claude Code (VSCode)
```
"Read product_context_engineering/00_project_context/ and PRD-001.
Let's implement the authentication feature."
```

Claude Code reads files directly from your workspace - no copy-paste needed.

### With Gemini CLI
```bash
# Inject context via pipe
cat product_context_engineering/00_project_context/*.md \
    product_context_engineering/04_product_requirements/PRD-001.md \
| gemini "implement the authentication feature"

# Or create an alias
alias pce-context="cat product_context_engineering/00_project_context/*.md"
pce-context | gemini "suggest next feature to build"
```

**→ See [FRAMEWORK_GUIDE.md](./FRAMEWORK_GUIDE.md) for detailed workflows and patterns**

---

## What Makes PCE Different?

| Without PCE | With PCE |
|-------------|----------|
| ❌ Re-explain vision every AI session | ✅ AI reads `project_brief.md` |
| ❌ AI violates constraints frequently | ✅ Constraints in `system_patterns.md` enforced |
| ❌ Decisions lost in chat history | ✅ Tracked in `working_docs/`, promoted when mature |
| ❌ Switching AI tools = start from scratch | ✅ Point new tool to context files |
| ❌ Context scattered across tools | ✅ Single source of truth in Git |

---

## Who Is This For?

**Primary**: Product Managers transitioning to AI-first development workflows
- Already familiar with PRDs, user stories, roadmaps
- Struggling with LLM inconsistency, context loss, constraint violations
- Need systematic approach to govern AI agents
- Working solo or in small teams (1-10 people)

**Also useful for**:
- Solo founders using AI to build products
- Engineering teams adopting AI coding assistants
- Anyone doing "vibe coding" who wants product rigor

---

## Documentation

- **[Getting Started](./GETTING_STARTED.md)** - Step-by-step setup guide (15 minutes)
- **[Framework Guide](./FRAMEWORK_GUIDE.md)** - Planning Mode, Act Mode, and workflows
- **[Troubleshooting](./TROUBLESHOOTING.md)** - Common issues and solutions
- **[Glossary](./GLOSSARY.md)** - Framework terminology

---

## Community & Contributions

PCE is open-source and community-driven. We welcome:

- 🌟 **Sharing your implementation** - Show us how you use PCE
- 📖 **Documentation improvements** - Found something unclear? PR welcome
- 🐛 **Issue reports** - Template doesn't work for your use case? Tell us
- 💡 **Enhancement suggestions** - New template ideas, workflow improvements

Open an issue or submit a PR on [GitHub](https://github.com/yourusername/product_context_engineering)

---

## Credits

Created by [Victor Dias](https://www.linkedin.com/in/victordias0027/) as a systematic approach to "vibe coding with product management vision."

Born from building [Apolus.ai](https://apolus.ai) - a legal tech platform built entirely with AI-driven development while maintaining product rigor.

---

**PCE in one sentence**: Treat your product context like code—structured, versioned, and consumable by any AI agent—so you can build fast without losing your product vision.
