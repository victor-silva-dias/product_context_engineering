# Meeting Notes / Insight Log

<!--
PCE → Traditional PM Mapping:
This file = Your meeting notes + decision log
Purpose: Capture insights from discussions before they're forgotten
Update frequency: After every significant meeting or discovery
Workflow: Capture here → Promote to proper layer when mature → Delete

Think of this as your "inbox" for product context.
-->

**Date**: [YYYY-MM-DD]
**Topic**: [Subject]
**Participants**: [Names]

<!-- EXAMPLE:
**Date**: 2025-01-15
**Topic**: Authentication Strategy Discussion
**Participants**: Victor (PM), Sarah (Eng Lead), Marcus (Design)
-->

## 1. Key Takeaways
* [Insight 1]
* [Insight 2]

<!-- EXAMPLE:
* Users are abandoning signup due to lengthy verification process (45% drop-off at email verification step)
* Current session management is causing logout issues on mobile browsers
* Competitor X just launched magic link login - users expect this now
-->

## 2. Decisions Made
* [Decision A] -> [Impact/Action]

<!-- EXAMPLE:
* Using JWT tokens instead of sessions -> Simpler for mobile apps, update tech_stack.md
* Implementing magic link login for MVP -> Reduces friction, add to PRD-001
* Removing email verification for initial signup -> Trust & verify later, update user flow
-->

## 3. Action Items
* [ ] Task 1 (Owner)
* [ ] Task 2 (Owner)

<!-- EXAMPLE:
* [ ] Update 00_project_context/tech_stack.md with JWT decision (Victor)
* [ ] Create PRD-002-magic-link-login.md (Victor)
* [ ] Design magic link email template (Marcus)
* [ ] Spike: JWT implementation with refresh tokens (Sarah - 2 days)
-->

## 4. AI Context Update
* *Did this meeting change our Strategy or Requirements?*
* [Yes/No] -> If Yes, please update `03_strategy` or `04_requirements` immediately.

<!-- EXAMPLE:
* Yes - Authentication approach changed significantly
* Actions taken:
  - ✅ Updated tech_stack.md (JWT decision)
  - ✅ Created PRD-002-magic-link-login.md
  - ⏳ Will update strategy_doc.md once we validate magic link with users
-->
