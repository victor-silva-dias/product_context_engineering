# Technical Stack & Constraints

<!--
PCE → Traditional PM Mapping:
This file = Your tech specs + architecture decision records (ADRs)
Purpose: Give AI the approved tools and constraints so it doesn't suggest random technologies
Update frequency: Monthly or when major tech decisions change
-->

## 1. Core Stack
* **Language**: [e.g., Python / TypeScript]

<!-- EXAMPLE:
**Language**: TypeScript 5.3+
Why: Type safety prevents runtime errors, great IDE support for AI coding tools
-->

* **Framework**: [e.g., FastAPI / Next.js]

<!-- EXAMPLE:
**Framework**: Next.js 14 (App Router)
Why: React ecosystem, serverless-friendly, great DX
Not using: Pages Router (deprecated), create-react-app (unmaintained)
-->

* **Database**: [e.g., Postgres / Supabase]

<!-- EXAMPLE:
**Database**: PostgreSQL 16 + Drizzle ORM
Why: Drizzle is lightweight, type-safe, and doesn't hide SQL
Not using: Prisma (too magical), raw SQL (maintenance hell)
-->

---

## 2. AI & LLM Infrastructure
* **Orchestration**: [e.g., LangChain / LangGraph / LiteLLM]

<!-- EXAMPLE:
**Orchestration**: LangGraph (not LangChain)
Why: More control over flow, easier debugging, better for agentic workflows
Not using: LangChain Chains (too black-box for our use case)
-->

* **Models**:
    * *Reasoning*: [e.g., GPT-4o / Claude 3.5 Sonnet] - Use for complex logic.

<!-- EXAMPLE:
*Reasoning*: Claude 3.5 Sonnet via Anthropic API
Use when: Complex analysis, code generation, multi-step reasoning
Cost: ~$3/1M input tokens, $15/1M output tokens
-->

    * *Speed*: [e.g., GPT-4o-mini] - Use for simple classification.

<!-- EXAMPLE:
*Speed*: GPT-4o-mini via OpenAI API
Use when: Classification, simple extraction, batch processing
Cost: ~$0.15/1M input tokens, $0.60/1M output tokens
Rule: If task takes <10 words to describe, use mini model
-->

* **Vector DB**: [e.g., Pinecone / Pgvector]

<!-- EXAMPLE:
**Vector DB**: Pgvector (PostgreSQL extension)
Why: Same DB as main data, simpler ops, good enough for <1M vectors
When to switch: If we hit >1M vectors or need specialized features, migrate to Pinecone
-->

---

## 3. Architecture Principles
* **[Principle 1]**: [e.g., "AI First" - Assume the AI is the primary operator.]

<!-- EXAMPLE:
**API-First**: All features exposed as API endpoints before UI
Why: Enables CLI tools, integrations, and AI automation
Rule: If it can't be done via API, it shouldn't exist
-->

* **[Principle 2]**: [e.g., "Stateless" - Functions must be pure where possible.]

<!-- EXAMPLE:
**Stateless Functions**: Pure functions wherever possible, side effects isolated
Why: Easier to test, easier to reason about, easier for AI to modify
Exception: Database writes, external API calls (but wrap in pure logic layer)
-->

<!-- ADDITIONAL EXAMPLES:

**Boring Technology**: Prefer proven tools over bleeding edge
Why: Stability > coolness. AI has more training data on mature tech.
Rule: New tech requires written justification in working_docs

**Monorepo**: All code in single repo
Why: Easier refactoring, shared types, simpler deployment
Structure: /apps (frontend, backend), /packages (shared libs)

**Feature Flags**: New features behind flags, gradual rollout
Why: Deploy anytime, enable when ready, easy rollback
Tool: PostHog feature flags (built-in analytics)
-->

---

## 4. Approved Libraries & Tools

### Frontend
<!-- EXAMPLE:
- UI Components: shadcn/ui (not Material-UI, not Chakra)
- State Management: Zustand (not Redux, not Context for complex state)
- Forms: React Hook Form + Zod (not Formik)
- Styling: Tailwind CSS (no CSS-in-JS, no Sass)
- Icons: Lucide React (not Font Awesome)

Why these choices:
- shadcn: Copy-paste components, full control, no npm bloat
- Zustand: Simple API, no boilerplate, works with React Server Components
- RHF + Zod: Type-safe forms, great DX
- Tailwind: Utility-first, fast iteration, works well with AI code gen
-->

### Backend
<!-- EXAMPLE:
- Validation: Zod (shared between frontend and backend)
- Testing: Vitest (not Jest - faster, better ESM support)
- API Client: tRPC (not REST - end-to-end type safety)
- Auth: Clerk (not NextAuth - better DX, handles edge cases)

Why these choices:
- Zod: Single schema for FE validation and BE validation
- Vitest: Compatible with Vite, faster, modern
- tRPC: TypeScript all the way, no API drift
- Clerk: Focus on product, not auth infrastructure
-->

### DevOps
<!-- EXAMPLE:
- Hosting: Vercel (frontend + serverless functions)
- Database: Neon (serverless Postgres)
- Monitoring: Sentry (errors) + PostHog (analytics)
- CI/CD: GitHub Actions

Not using:
- AWS/GCP/Azure directly (too complex for small team)
- Docker/Kubernetes (overkill for our scale)
- Self-hosted anything (want to focus on product)
-->

---

## 5. Environment Variables

<!-- EXAMPLE:
```env
# AI/LLM
ANTHROPIC_API_KEY=sk-ant-...
OPENAI_API_KEY=sk-...

# Database
DATABASE_URL=postgresql://...
DATABASE_POOL_MAX=20

# Auth
CLERK_SECRET_KEY=sk_live_...
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_live_...

# Analytics
NEXT_PUBLIC_POSTHOG_KEY=phc_...
SENTRY_DSN=https://...
```

Rules for env vars:
- Never commit .env to Git (use .env.example)
- Prefix public vars with NEXT_PUBLIC_
- Store secrets in Vercel/deployment platform
- Document every var in .env.example
-->

---

## 6. Constraints & Forbidden Patterns

### ❌ Never Use
<!-- EXAMPLE:
- `any` type in TypeScript (use `unknown` if truly dynamic)
- `eval()` or `Function()` constructor
- Inline styles (use Tailwind classes)
- Global CSS (scope with CSS modules if needed)
- `var` (use `const` or `let`)
- Nested ternaries (if >2 levels, use if/else)
-->

### ✅ Always Prefer
<!-- EXAMPLE:
- Async/await over .then() chains
- Array methods (.map, .filter) over for loops
- Functional programming over OOP (where reasonable)
- Explicit error handling (no silent failures)
- Named exports over default exports
-->

---

## Notes for AI

When generating code for this stack:
- Check this file BEFORE suggesting libraries
- If you want to use a library not listed here, ask first
- Respect the architecture principles (they're non-negotiable)
- When in doubt about a pattern, check `system_patterns.md`

**Tech Debt We're Okay With** (don't "fix" these):
<!-- EXAMPLE:
- Using Vercel Edge Runtime (not Node.js) - simplifies deployment despite limitations
- No automated tests yet - will add when we have paying users
- Tailwind classes inline - we value speed over "separation of concerns"
-->

**Migration Plans** (future, don't implement now):
<!-- EXAMPLE:
- Q2 2026: Add automated testing (Vitest + Playwright)
- Q3 2026: Migrate to pgvector if hit vector limits
- TBD: Consider moving to monorepo if we split frontend/backend teams
-->
