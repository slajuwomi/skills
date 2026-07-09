---
name: adversarial-review
description: Run a Bun/Jarred-Sumner-style adversarial code review — blind independent reviewers who assume a diff is broken, followed by skeptic agents that try to refute each finding before it survives. Use when the user says "adversarial review", "review this like Jarred did for Bun", "/adversarial-review", or wants a more rigorous multi-agent review than a single pass.
---

Two-round review of a diff: independent reviewers who never see why the change was made, then independent skeptics who try to kill each finding. Only findings that survive skepticism get reported.

## Process

### 1. Gather the diff

Default to `git diff` (unstaged) or `git diff --staged` if that's empty. If the user names a range, commit, or branch, use `git diff <that>...HEAD` instead. If there's nothing to diff, ask what to review — don't guess.

Confirm the diff is non-empty before spawning anything.

### 2. Blind review round

Send a single message with 2 `Agent` tool calls (subagent_type `general-purpose`, `run_in_background: false`). Each reviewer prompt contains **only the diff text** — no commit message, no issue link, no explanation of intent. Neither reviewer sees the other's output.

**Reviewer prompt** — include the raw diff, then this brief:

> You are an adversarial code reviewer. Assume this diff is broken — your only job is to find reasons it fails, not to confirm it works. You were not told why this change was made and have no other context; review only what's in front of you.
>
> For each problem you find, report: `file:line` — one-sentence summary — the exact input, state, or sequence of calls that triggers the failure. Include logic bugs, edge cases (empty/null/negative/concurrent), resource leaks, and mismatches between what the code does and what it looks like it's trying to do. Skip style nits and anything a linter would catch.
>
> Return a plain list, one finding per line. If you find nothing, say "No findings." Do not hedge — if you're not sure it's a real bug, say so in the summary rather than omitting it. Under 300 words.

### 3. Dedup

Merge the two reviewers' findings yourself. Two findings collapse into one candidate if they name the same `file:line` (or immediately adjacent lines) and describe the same underlying problem, even if worded differently — this is a judgment call, not a string match.

### 4. Skeptic verification round

For each surviving candidate, send a single message with 3 `Agent` tool calls (subagent_type `general-purpose`), each given only that one finding plus the relevant diff hunk, instructed to independently try to **refute** it — argue why it's not actually a bug, a false positive, or already handled elsewhere in the diff. Tell each skeptic to default to `refuted: true` if genuinely unsure, so uncertainty doesn't accidentally save a weak finding.

A finding survives only if a **majority (2 of 3) do not refute it**. Track the vote count per finding — it goes in the report.

### 5. Report via lavish

Build an HTML report (`.lavish/adversarial-review.html`) using the `lavish` skill's `table` and `code` playbooks — open both playbooks before writing HTML, per `lavish/SKILL.md`. One row per confirmed finding: `file:line`, one-line problem, failure scenario, skeptic vote (e.g. "2/3 disagreed"), with the relevant diff hunk rendered alongside via the code playbook. Then `npx -y lavish-axi <html-file>` to open it.

Note the refuted count (e.g. "3 findings raised, 1 confirmed, 2 refuted by skeptics") so nothing is silently dropped — but keep refuted findings out of the main table, in a collapsed/secondary section if the layout supports it.

## Boundary

Report-only. This skill never applies a fix, and never auto-approves or auto-merges anything — the human reads every confirmed finding and decides, the same way Jarred read every subagent's output before it shipped.

"stop adversarial review" or a plain review request: fall back to a normal single-pass review instead.
