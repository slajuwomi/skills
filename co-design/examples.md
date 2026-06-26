# Co-Design Examples

## Example 1: New feature in existing app

**User:** `co-design — I want to add a notification system to this app`

**Expected agent behavior:**

1. Explore the codebase — existing user model, real-time infra, email setup
2. Frame the problem: what triggers notifications, who receives them, delivery channels
3. Discuss trade-offs over 2–4 turns:
   - In-app vs email vs push
   - Polling vs WebSockets vs SSE
   - Database schema for notifications table
4. After each area, summarize **"In plain terms"**
5. Wait for user approval before writing the plan
6. Write `plans/notification-system.md`

**Sample conversation turn:**

> **Recommendation:** Start with in-app notifications stored in Postgres, delivered via polling every 30s. Add email for critical alerts only.
>
> **Alternatives:** WebSockets (real-time but adds infra complexity), email-only (simpler but poor UX for non-critical updates).
>
> **In plain terms:** Users see a bell icon with unread count. The app checks for new notifications periodically. Important alerts (password reset, payment failed) also send email.

---

## Example 2: New product (greenfield)

**User:** `new project — a recipe sharing app with social features`

**Expected agent behavior:**

1. Scope MVP vs later features with the user
2. Recommend stack with trade-offs (e.g. Next.js + Postgres vs mobile-first)
3. Walk through: auth, recipe CRUD, social feed, image storage, search
4. Confirm understanding on each major decision
5. Write `plans/recipe-sharing-app.md` with phased implementation

**Plan should include:**

- MVP feature list vs v2
- Schema: users, recipes, follows, likes, comments
- Frontend: pages and component structure
- Phase 1: auth + recipe CRUD
- Phase 2: social feed
- Phase 3: search and discovery

---

## Example 3: User asks a question mid-design

**User:** `Why Postgres over MongoDB for this?`

**Expected agent behavior:**

- Answer clearly with jargon defined inline
- Relate the answer to *this specific feature* (not generic database comparison)
- Ask if the user wants to revisit the decision or continue
- Do not jump to implementation

---

## Trigger phrases

- `@co-design`
- `co-design`, `co design`, `codesign`
- `planning`, `plan mode`, `plan this`
- `new project`, `new feature`, `greenfield`

## What co-design does NOT do

- Write production code (unless user asks for a sketch to clarify a decision)
- Rush to a plan without back-and-forth
- Overwhelm with questions (contrast with grill-me's one-at-a-time relentless style)
- Skip the approval step before writing `plans/<slug>.md`
