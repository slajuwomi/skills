# Solo Learning Examples

## Example 1: Bug fix request

**User:** `@solo-learning my form validation isn't working — fix it`

### Good response

```markdown
**Problem:** You need to understand why client-side validation isn't blocking invalid submissions in your form.

**Read first**
- [Constraint validation API (MDN)](https://developer.mozilla.org/en-US/docs/Web/API/Constraint_validation) — focus on `checkValidity()` and `reportValidity()`
- [HTML form validation (MDN)](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation) — focus on when the browser validates vs when JS must intervene

**Search**
- [html form validation not firing on submit react](https://www.google.com/search?q=html+form+validation+not+firing+on+submit+react)

**In this repo**
- `src/components/ContactForm.tsx` — look at the submit handler and how the `<form>` element is wired
- `src/lib/validation.ts` — look at `validateContactFields`

**Work through these**
1. Does your submit handler call `preventDefault()` before or after validation runs?
2. Are you relying on native HTML constraints (`required`, `pattern`) or only custom JS — and do both paths agree?
3. When you log the form's `validity` object on submit, which field reports invalid?

---
Stuck? Say **reveal answer** or use `@learning-mode`.
```

### Bad response (do not do this)

> The issue is you're calling `preventDefault()` before `checkValidity()`. Add `if (!form.checkValidity()) return` at the top of your handler.

This gives away the fix instead of guiding discovery.

---

## Example 2: Concept question

**User:** `@solo-learning how does React useEffect cleanup work?`

### Good response

```markdown
**Problem:** You want to understand what the function returned from `useEffect` does and when React runs it.

**Read first**
- [Synchronizing with Effects — React docs](https://react.dev/learn/synchronizing-with-effects) — focus on "How to handle two common Issues" and cleanup
- [useEffect reference — React docs](https://react.dev/reference/react/useEffect) — focus on the cleanup function section

**Search**
- [react useEffect cleanup function when does it run](https://www.google.com/search?q=react+useEffect+cleanup+function+when+does+it+run)

**Work through these**
1. What triggers the cleanup function to run — component unmount, dependency change, or both?
2. If your effect sets up a subscription or timer, what happens if you skip returning a cleanup function?
3. How does Strict Mode in development affect when cleanup runs compared to production?

---
Stuck? Say **reveal answer** or use `@learning-mode`.
```

### Bad response (do not do this)

> Cleanup runs before the effect re-runs and on unmount. It's like `componentWillUnmount` for that specific effect — you return a function that tears down subscriptions.

This explains the concept instead of pointing the user to primary sources.

---

## Example 3: Escape hatch

**User:** `@solo-learning why is my fetch returning undefined?`

**Agent (solo-learning):** [delivers template response with fetch docs, repo pointer to `api/client.ts`, Socratic questions]

**User:** `reveal answer`

### Good response after escape hatch

> Switching to learning mode.
>
> **Hint:** Look at whether you're awaiting the fetch call and whether you're returning the parsed JSON from the async function — not the Promise itself. In `api/client.ts`, trace what `getUser()` actually returns to its caller.
>
> Want hands-on practice? I can set up a Learn by Doing exercise around the async flow in that file.

### Bad response after escape hatch

> Still in solo-learning: try reading the MDN fetch docs again.

Once the user invokes the escape hatch, follow `@learning-mode` — hints and Learn by Doing are allowed.
