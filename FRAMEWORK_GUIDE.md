# Framework Guide

**How to use Product Context Engineering with vibe coding agents (Claude Code, Gemini CLI).**

---

## The Core Philosophy: Vibe Coding with Context

PCE enables **pure vibe coding** — rapid AI-assisted development grounded in product context. Instead of rigid processes, you simply tell AI to read your context folder and build.

**The Magic Prompt Pattern**:
```
"Read product_context_engineering/ and let's [build the product vision / create feature X / refine the strategy]"
```

That's it. AI reads your context, understands your product, and helps you build — whether you're doing product work (Planning Mode) or development work (Act Mode).

---

## Two Modes: Planning → Act

### Planning Mode (Discovery)

**What**: Use AI to CREATE your product vision, strategy, and PRDs
**Input**: Existing context + working_docs insights + your application/product code
**Output**: Updated context files (vision, PRDs, strategy documents)

**Example Prompts**:
```
"Read product_context_engineering/ and working_docs/, analyze our current product code,
and let's refine our product vision based on what we've learned."

"Read the context folder and help me create a PRD for the user authentication feature
we discussed in working_docs/meeting_notes.md"

"Based on our current codebase and context, help me define our Q1 2026 strategy."
```

**Key Insight**: Product work IS vibe coding. You're not just documenting decisions — you're using AI to help make them, generate insights, and create strategy.

---

### Act Mode (Delivery)

**What**: Use AI to CODE features with documented context
**Input**: Context files + PRD for specific feature
**Output**: Working code that aligns with vision and constraints

**Example Prompts**:
```
"Read product_context_engineering/ and let's implement the login feature from PRD-001."

"Read the context folder and help me refactor the API layer to match our system_patterns."

"Based on our tech_stack.md and system_patterns.md, generate the user profile component."
```

**Key Insight**: AI codes with full product understanding — no drift, no misalignment.

---

## Recommended Tools for Vibe Coding

PCE works with **workspace-aware AI agents** - tools that can read files from your project.

### What You Need

**IDE**: VSCode, Cursor, or similar
**Vibe Coding Agent**: Any AI assistant with workspace file access
- Claude Code (VSCode extension)
- Gemini CLI
- Cursor built-in AI
- Similar tools that can read project files

**Version Control**: Git/GitHub (context files live alongside code)

---

### Why Workspace-Aware Agents?

The key requirement is simple: **your AI agent needs to read files from your project**.

This allows the simple pattern:
- "Read product_context_engineering/" → AI reads the files
- No copy-paste, no configuration, no complex setup
- Context lives in Git, works across different AI tools

---

## How to Work with Planning and Act Modes

The framework operates in two distinct modes. You switch between them based on what you're trying to accomplish.

### Planning Mode (Discovery)

**Purpose**: Create and refine your product context - the vision, strategy, and requirements that guide development.

**When to use**:
- Defining or updating product vision
- Creating PRDs for new features
- Refining strategy and roadmap
- Making architectural decisions
- Documenting constraints and patterns

**What you do**:
1. Tell your AI agent to read existing context + working docs
2. Ask it to help refine vision, create PRDs, or define strategy
3. AI analyzes your current codebase and context
4. Together you generate/update context files
5. Commit the context changes to Git

**Key insight**: Product work IS vibe coding. You're using AI to help think through strategy, not just document it.

**Example session**:
- "Read our context folder and working docs. Based on what we've built so far, help me refine our product vision."
- "Read the context and help me create a PRD for user authentication."
- "Based on our codebase, what should our Q1 priorities be?"

---

### Act Mode (Delivery)

**Purpose**: Build features with full product context already defined.

**When to use**:
- Implementing features from PRDs
- Refactoring code to match patterns
- Building new components
- Any coding work where requirements are clear

**What you do**:
1. Tell your AI agent to read the relevant context (vision, tech stack, patterns, specific PRD)
2. Ask it to implement the feature
3. AI generates code aligned with your product strategy
4. AI follows constraints from system_patterns.md
5. Commit the code changes to Git

**Key insight**: AI codes with full product understanding - no drift, no misalignment.

**Example session**:
- "Read project context and PRD-001. Let's implement the authentication feature."
- "Read our system patterns and refactor the API layer to match."
- "Based on tech stack and PRD-002, build the user profile component."

---

## The Simple Pattern

Regardless of which AI agent you use, the pattern is the same:

**1. Point AI to context**
- "Read product_context_engineering/[relevant folders]"

**2. State your goal**
- Planning Mode: "help me create/refine [vision/PRD/strategy]"
- Act Mode: "let's implement [feature from PRD]"

**3. Work together**
- AI reads context, understands product, generates output
- You review, refine, approve

**4. Commit to Git**
- Context changes (Planning Mode) or code changes (Act Mode)

That's it. No complex setup, no configuration files, no custom prompts needed.

---

## Best Practices

### ✅ Do
- **Version all context in Git** - Context files are code
- **Use the simple prompt pattern** - "Read product_context_engineering/ and..."
- **Keep working_docs as inbox** - Capture → Promote → Delete
- **Update context continuously** - If it's not in a file, it doesn't exist
- **Commit context changes separately** - Track product decisions over time

### ❌ Don't
- **Rely on chat history** - Context amnesia when session ends
- **Skip PRDs for "quick features"** - Technical debt starts here
- **Let working_docs pile up** - Promote mature decisions to proper layers
- **Forget to commit context** - Product decisions should be versioned like code

---

## Troubleshooting

**"AI still ignores my constraints"**
→ See [TROUBLESHOOTING.md](./TROUBLESHOOTING.md#ai-keeps-ignoring-my-constraints)

**"Context files are too large"**
→ See [TROUBLESHOOTING.md](./TROUBLESHOOTING.md#context-files-are-getting-too-large)

**"Not sure when to create PRD vs just code"**
→ See [TROUBLESHOOTING.md](./TROUBLESHOOTING.md#not-sure-when-to-create-prd-vs-just-code)

---

## Typical Workflow

Here's how Planning and Act modes work together in practice:

**Week 1 - Planning Mode**
1. Create initial context (project_brief, tech_stack, system_patterns)
2. Use AI to help refine vision and identify first features
3. Create PRDs for priority features
4. Commit context to Git

**Week 2-4 - Act Mode**
1. Pick a PRD to implement
2. Tell AI: "Read context and PRD-001, let's build this"
3. AI codes with full product understanding
4. Ship feature, commit code

**Ongoing - Both Modes**
- Daily: Capture insights in working_docs/
- Weekly: Use Planning Mode to promote decisions, create new PRDs
- Sprint by sprint: Act Mode to build, Planning Mode to refine

**The rhythm**: Planning Mode creates the "what" and "why". Act Mode executes the "how".

---

## Common Questions

**Q: When should I use Planning vs Act Mode?**
A: If you're thinking about product (vision, strategy, requirements) → Planning Mode. If you're ready to code → Act Mode.

**Q: Can I switch between modes in the same session?**
A: Yes! "Let's refine this PRD (Planning), then implement it (Act)."

**Q: Do I need different AI agents for each mode?**
A: No. The same agent works for both - just change what you ask it to do.

**Q: How much context should I inject?**
A: Start minimal (00_project_context only). Add more layers (PRDs, strategy) as needed for the specific task.

---

## Remember

PCE is vibe coding with product rigor:
- **Planning Mode** ensures you're building the right thing
- **Act Mode** builds it fast without losing product vision
- **Context files** are the bridge between modes
- **Git** versions both product decisions and code together

The framework doesn't prescribe how to use your AI agent - it gives you a structure that works with any workspace-aware tool.
