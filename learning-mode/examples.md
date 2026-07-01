# Learning Mode Examples

## Example 1: Without guide

**User:** `learning mode — help me add auth to this Express app`

**Expected agent behavior:**

1. Explore the existing Express app structure
2. Set up auth scaffolding (routes, middleware placeholders)
3. Insert `TODO(human)` for a meaningful decision (e.g. session vs JWT strategy)
4. Issue a Learn by Doing request and stop
5. After user implements their piece, share one insight connecting their choice to broader patterns

**Sample Learn by Doing block:**

```
• **Learn by Doing**

**Context:** I've set up the Express auth routes and middleware skeleton. The login endpoint receives credentials and calls a placeholder `validateUser()` function. The critical decision is how to keep the user logged in after a successful login.

**Your Task:** In auth/middleware.js, implement the `createSession(userId)` function. Look for TODO(human). It should create a session token and return it.

**Guidance:** Consider trade-offs between storing sessions in memory (simple, lost on restart) vs a database/Redis store (persistent, scales across servers). Match whatever pattern you see elsewhere in the codebase if one exists.

**Look up:** "Express session store options"
```

---

## Example 2: With guide

**User:** `learning mode guide — teach me React Server Components in this Next.js app`

**Expected agent behavior:**

1. Write `guides/react-server-components.html` using guide-template.html
2. Tell the user the file path
3. Continue with hands-on learning in the Next.js codebase

**Guide should include:**

- Plain-language overview of Server vs Client Components
- Definitions for terms like "hydration", "RSC payload", "use client"
- A worked example using patterns from the user's Next.js version
- Links to official Next.js and React docs (verified URLs only)

---

## Example 3: Debugging exercise

**User:** `@learning-mode the form validation isn't working, help me debug it`

**Expected agent behavior:**

1. Investigate the validation code
2. Set up context around the suspected function
3. Ask user to add debug logging via Learn by Doing (not fix it for them)
4. After user adds logs and shares results, guide interpretation with insights

---

## Trigger phrases

- `@learning-mode`
- `learning mode`
- `learn by doing`
- `teach me while we build`
- `I want to understand, not just copy code`
- Include `guide` for HTML guide generation first
