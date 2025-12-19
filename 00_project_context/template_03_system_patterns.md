# System Patterns & Design Guidelines

<!--
PCE → Traditional PM Mapping:
This file = Your code style guide + design system + architecture patterns
Purpose: Give AI the "how" of implementation so code is consistent
Update frequency: When patterns emerge or change (monthly typically)
-->

## 1. Code Style
* **Formatting**: [e.g., Black / Prettier]

<!-- EXAMPLE:
**Formatting**: Prettier (default config, no customization)
Why: Zero config, ubiquitous, works with all editors
Run: `npm run format` before commit (enforced by pre-commit hook)
-->

* **Naming Conventions**: [e.g., snake_case for Python, camelCase for JS]

<!-- EXAMPLE:
**Naming Conventions**:
- Files: `kebab-case.ts` (e.g., `user-profile.ts`)
- Components: `PascalCase.tsx` (e.g., `UserProfile.tsx`)
- Functions/variables: `camelCase` (e.g., `getUserProfile`)
- Constants: `UPPER_SNAKE_CASE` (e.g., `MAX_RETRIES`)
- Types/Interfaces: `PascalCase` (e.g., `UserProfile`, `ApiResponse`)
- CSS classes: Tailwind utilities only (no custom class names)

Why these choices:
- Follows community conventions (TypeScript/React)
- AI generates code in these patterns naturally
- Easier to find files (all component files end in .tsx)
-->

* **Type Hinting**: [Strict typing required?]

<!-- EXAMPLE:
**Type Hinting**: Strict TypeScript, no implicit any
- tsconfig.json: `"strict": true`, `"noImplicitAny": true`
- Every function has explicit return type
- Use `unknown` instead of `any` for truly dynamic data
- Zod schemas for runtime validation (complements TypeScript)

Exception: Test files can be looser (test data doesn't need full typing)
-->

---

## 2. Directory Structure Pattern
```
src/
    agents/       # AI Logic
    core/         # Business Logic
    interfaces/   # API / UI
```

<!-- EXAMPLE:
```
src/
├── app/                    # Next.js App Router (pages)
│   ├── (auth)/            # Route group (auth required)
│   ├── (marketing)/       # Route group (public)
│   ├── api/               # API routes
│   └── layout.tsx         # Root layout
├── components/            # React components
│   ├── ui/               # shadcn components (generated)
│   └── features/         # Feature-specific components
├── lib/                  # Shared utilities
│   ├── db/              # Database client, schemas
│   ├── api/             # tRPC routers
│   └── utils/           # Helper functions
├── types/               # TypeScript types (global)
└── middleware.ts        # Auth/routing middleware
```

Rules:
- UI components in /components, business logic in /lib
- No business logic in /app (just routing and data fetching)
- Shared types in /types, feature-specific types colocated
- Tests colocated with source files (user-profile.ts → user-profile.test.ts)
-->

---

## 3. Error Handling Strategy
* [Define how errors should be logged and surfaced to the user/AI]

<!-- EXAMPLE:
**Error Handling**: Explicit, typed, never silent

1. **Never swallow errors**:
   ```ts
   // ❌ Bad - silent failure
   try { await riskyOperation() } catch (e) {}

   // ✅ Good - log or rethrow
   try {
     await riskyOperation()
   } catch (error) {
     logger.error('Risky operation failed', { error })
     throw new AppError('Operation failed', { cause: error })
   }
   ```

2. **Use custom error classes**:
   ```ts
   // Custom errors with metadata
   export class NotFoundError extends Error {
     constructor(resource: string, id: string) {
       super(`${resource} not found: ${id}`)
       this.name = 'NotFoundError'
     }
   }
   ```

3. **API errors return structured format**:
   ```ts
   {
     "error": {
       "code": "VALIDATION_ERROR",
       "message": "User-friendly message",
       "details": { "field": "email", "issue": "invalid format" }
     }
   }
   ```

4. **UI errors show toast, log to Sentry**:
   - User sees: "Failed to save. Please try again."
   - Sentry logs: Full stack trace + user context

**Forbidden**:
- Generic error messages ("Something went wrong")
- Throwing strings instead of Error objects
- console.error in production (use logger)
-->

---

## 4. Validated Design Patterns
* **[Pattern Name]**: [Description of a pattern that works well for this project]

<!-- EXAMPLE:
**Server Actions for Mutations**: Use Next.js Server Actions, not API routes

Why:
- Type-safe (no API contract drift)
- Less boilerplate (no fetch/axios code)
- Better error handling (can return typed errors)

Pattern:
```ts
// actions/user.ts
'use server'

import { z } from 'zod'

const updateProfileSchema = z.object({
  name: z.string().min(1),
  email: z.string().email(),
})

export async function updateProfile(data: z.infer<typeof updateProfileSchema>) {
  // Validate
  const parsed = updateProfileSchema.parse(data)

  // Auth check
  const user = await getUser()
  if (!user) throw new UnauthorizedError()

  // Mutation
  await db.update(users).set(parsed).where(eq(users.id, user.id))

  // Return success (no need for JSON response formatting)
  return { success: true }
}
```

Usage in component:
```tsx
import { updateProfile } from '@/actions/user'

const handleSubmit = async (data) => {
  const result = await updateProfile(data)
  if (result.success) toast.success('Saved!')
}
```
-->

<!-- ADDITIONAL PATTERN EXAMPLES:

**Zod for Data Validation**: Use Zod schemas, share between FE/BE

Why:
- Single source of truth for validation rules
- Type inference (TS types generated from schemas)
- Runtime validation (TypeScript only checks at build time)

Pattern:
```ts
// schemas/user.ts
import { z } from 'zod'

export const userSchema = z.object({
  id: z.string().uuid(),
  email: z.string().email(),
  name: z.string().min(1).max(100),
  createdAt: z.date(),
})

export type User = z.infer<typeof userSchema>

// Use in API response validation
const response = await fetch('/api/user')
const data = await response.json()
const user = userSchema.parse(data) // Runtime check + TS type
```

---

**React Hook Form + Zod**: Standard form pattern

Why:
- Type-safe forms
- Built-in validation
- Great DX with devtools

Pattern:
```tsx
import { useForm } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'
import { loginSchema } from '@/schemas/auth'

function LoginForm() {
  const form = useForm({
    resolver: zodResolver(loginSchema),
    defaultValues: { email: '', password: '' },
  })

  const onSubmit = form.handleSubmit(async (data) => {
    await login(data)
  })

  return (
    <form onSubmit={onSubmit}>
      <input {...form.register('email')} />
      {form.formState.errors.email && <span>{form.formState.errors.email.message}</span>}
    </form>
  )
}
```

---

**Database Queries with Drizzle**: Type-safe SQL builder

Pattern:
```ts
import { db } from '@/lib/db'
import { users, posts } from '@/lib/db/schema'
import { eq, and } from 'drizzle-orm'

// Simple query
const user = await db.query.users.findFirst({
  where: eq(users.id, userId),
})

// Join with type inference
const postsWithAuthors = await db
  .select()
  .from(posts)
  .innerJoin(users, eq(posts.authorId, users.id))
  .where(eq(posts.published, true))

// Full type safety - TypeScript knows the shape of result
```

---

**Loading States with Suspense**: Use React Suspense boundaries

Why:
- Declarative loading states
- Works with Server Components
- No manual loading flags

Pattern:
```tsx
import { Suspense } from 'react'
import { Skeleton } from '@/components/ui/skeleton'

export default function Page() {
  return (
    <Suspense fallback={<Skeleton className="h-20 w-full" />}>
      <UserProfile />
    </Suspense>
  )
}

// UserProfile can be async Server Component
async function UserProfile() {
  const user = await getUser() // Suspends until resolved
  return <div>{user.name}</div>
}
```

-->

---

## 5. File Organization Rules

### Maximum File Sizes
<!-- EXAMPLE:
- Components: Max 200 lines (if longer, split into sub-components)
- Utilities: Max 150 lines (if longer, split by concern)
- API routes: Max 100 lines (if longer, extract logic to /lib)

Why: AI (and humans) struggle with files >200 lines
Exception: Generated files (e.g., shadcn components) can be longer
-->

### Colocate Related Files
<!-- EXAMPLE:
Instead of:
```
src/
├── components/
│   └── UserProfile.tsx
├── hooks/
│   └── useUserProfile.ts
└── utils/
    └── formatUserName.ts
```

Do:
```
src/
└── features/
    └── user-profile/
        ├── UserProfile.tsx
        ├── useUserProfile.ts
        ├── formatUserName.ts
        └── UserProfile.test.ts
```

Why: Related code changes together, easier to refactor
Exception: Truly shared utilities stay in /lib/utils
-->

---

## 6. Comments & Documentation

### When to Comment
<!-- EXAMPLE:
✅ **Do Comment**:
- Why (not what): "Using setTimeout because Clerk's session isn't ready immediately"
- Complex algorithms: "Binary search for optimal pricing tier"
- Workarounds: "TODO: Remove after Vercel fixes Edge Runtime bug"
- Public API functions: JSDoc with param descriptions

❌ **Don't Comment**:
- Obvious code: `// Set user name` above `user.name = name`
- What the code does (code should be self-explanatory)
- Commented-out code (delete it, Git remembers)
-->

### Forbidden in Production
<!-- EXAMPLE:
- `TODO` or `FIXME` comments (create GitHub issue instead)
- `console.log` (use proper logger)
- Commented-out code (delete it)
- Placeholder comments like `// Will implement later`

Why: These create tech debt and clutter. If it's important, track it properly.
-->

---

## 7. Testing Guidelines

<!-- EXAMPLE:
**Testing Philosophy**: Test behavior, not implementation

What to test:
- ✅ User flows (can user log in?)
- ✅ Edge cases (what if email is invalid?)
- ✅ Business logic (discount calculation correct?)

What NOT to test:
- ❌ Implementation details (component uses useState?)
- ❌ Library behavior (React Router works?)
- ❌ Generated code (shadcn components?)

Tool stack:
- Unit/Integration: Vitest
- E2E: Playwright
- Coverage goal: >70% for critical paths (not 100%)

Pattern:
```ts
// user-profile.test.ts
import { describe, it, expect } from 'vitest'
import { updateProfile } from './actions'

describe('updateProfile', () => {
  it('should update user name', async () => {
    const result = await updateProfile({ name: 'New Name' })
    expect(result.success).toBe(true)
  })

  it('should reject invalid email', async () => {
    await expect(
      updateProfile({ email: 'invalid' })
    ).rejects.toThrow('Invalid email')
  })
})
```
-->

---

## Notes for AI

When writing code for this project:
- Read this file BEFORE `tech_stack.md` (patterns > tools)
- If a pattern isn't documented here, ask before creating new ones
- Consistency matters more than "best practices" from the internet
- When refactoring, maintain these patterns (don't introduce new ones)

**Code Review Checklist** (AI should validate):
- [ ] Follows naming conventions
- [ ] No files >200 lines
- [ ] Error handling is explicit (no silent catches)
- [ ] Types are strict (no `any`)
- [ ] Tests cover edge cases
- [ ] No TODO/FIXME in production code

**When Patterns Conflict**:
If this file contradicts `tech_stack.md`, this file wins (patterns > tools).
If you notice a conflict, flag it for review.
