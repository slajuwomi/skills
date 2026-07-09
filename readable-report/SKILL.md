---
name: readable-report
description: Generate a single self-contained static HTML report file (no server, no CLI, no polling) using one fixed light-mode editorial design system - warm cream background, near-black text, amber accent, Crimson Pro headings + Atkinson Hyperlegible body text. Write it directly with the Write/Edit tools; the user opens it in a browser, nothing to run. Use when the user wants a readable, printable, or shareable write-up, summary, recap, or report of a plan, decision, analysis, or comparison, and does NOT want live annotation or an interactive feedback loop (use the lavish skill instead for that). If a grill-me / grilling session happened earlier in this same conversation, this skill also compiles the full Q&A history into a static multiple-choice recap section with a "Copy my answers" button, so use it when the user says things like "write this up", "turn our Q&A into a doc", or asks for a recap after grilling.
argument-hint: <what the report should cover>
metadata:
  hermes:
    tags: [html, report, static, documentation]
    category: productivity
---

# Readable Report

Generate a static, self-contained HTML report the user opens directly in a
browser. No server, no CLI tool, no polling loop — write the file once with
the Write tool and you're done.

## Request

$ARGUMENTS

If non-empty, build a report for that request. If empty, infer what to
report on from the conversation (the plan, analysis, or decision just made).

## Workflow

1. Read `templates/report-template.html` in this skill's directory. It has
   the full design system (fonts, CSS tokens) already baked in — do not
   invent new colors, fonts, or spacing; the design is fixed on purpose.
2. Decide the report's sections from the content you're summarizing (e.g.
   Overview, Decisions, Trade-offs, Open Questions, Next Steps) — use `<h2>`
   per section inside `<main class="prose">`. Use `.card` divs to set apart
   key recommendations or callouts, `<table>` for structured comparisons.
   Keep prose inside the `.prose` container so line length stays readable.
   For any code snippet, wrap it as `<pre><code class="language-xxx">...</code></pre>`
   where `xxx` is the actual language (e.g. `language-js`, `language-python`,
   `language-bash`, `language-json`, `language-html`) so highlight.js (already
   wired up in the template) applies accurate syntax highlighting. Escape
   `<`, `>`, and `&` in the snippet text. If the language is unknown, omit
   the class and let highlight.js auto-detect.
3. Check whether a grilling session (grill-me / grill-with-docs / grilling
   skill's one-question-at-a-time interview pattern) happened earlier in
   THIS conversation. There is no file or state to read for this — the
   grilling skill has no persisted log, so you (the agent) must recall the
   full Q&A path from your own conversation context.
   - If no such session occurred: delete the entire
     `<!-- QA_SECTION_START -->` … `<!-- QA_SECTION_END -->` block and the
     `QA_SCRIPT` script block from the template.
   - If it did occur: keep both blocks, and inside `#qa-questions` add one
     `.qa-question` block (see template comments for exact markup) per
     question asked, in the order it was asked, covering the full path down
     the design tree — not just the last few questions. For each question:
     the recommended answer you gave becomes one radio option (tagged
     "recommended"), every other answer actually discussed becomes another
     plain option, and a `__custom__` radio + disabled text input is always
     added last. Check whichever option matches what the user actually
     decided (fall back to the recommended option only if that's genuinely
     unclear). Give each question's radio group a unique `name` (`q1`,
     `q2`, ...) so they don't collide.
4. Fill in `<title>`, `<h1>`, and the `.meta` line (e.g. today's date or a
   one-line description) in the header.
5. Save the file. Default location: `.readable-report/<slug>.html` in the
   current working directory (mirrors the `.lavish/<name>.html` habit from
   the lavish skill), where `<slug>` is a short kebab-case name for the
   report. Use a path the user explicitly specifies instead if they give one.
6. Open the finished file in the user's default browser automatically —
   run `open <path>` on macOS, `xdg-open <path>` on Linux, or
   `start "" <path>` on Windows (detect the platform; on macOS this repo's
   environment, `open` is correct). Do this without asking first; it's a
   local, reversible action (just opens a browser tab). Then tell the user
   the file path. If the report includes the Q&A recap section, also tell
   them: after reviewing or changing any answers in the browser, click
   "Copy my answers" and paste the result back into the chat here so you
   can continue from their decisions.

## When NOT to use this skill

If the user wants live annotation, a feedback loop, or to keep iterating on
the artifact interactively in the browser while talking to the agent, use
the `lavish` skill instead — that's what `lavish-axi` and its polling loop
are for. This skill is one-shot and static by design.

## Design system (fixed, do not adapt to the current repo)

Unlike some other skills, this report never matches the host project's
design system — it always uses the same warm-cream editorial theme:

| Token | Value | Use |
|---|---|---|
| `--bg` | `#FFFBEB` | page background |
| `--fg` | `#0F172A` | body/heading text |
| `--card` | `#FFFFFF` | card/callout backgrounds |
| `--accent` | `#D97706` | links, tags, the copy button, sparing emphasis only |
| `--border` | `#EEEDED` | hairline borders/dividers |
| `--muted` | `#78716C` | secondary/meta text |

Headings: "Crimson Pro" (serif). Body: "Atkinson Hyperlegible" (sans,
accessibility-optimized). Both loaded via Google Fonts in the template's
`<head>` — do not swap fonts. Body text stays ≥16px, line-height ~1.6,
prose max-width ~70ch, no shadows/gradients beyond the one subtle sticky
copy-bar shadow already in the template, single accent color used sparingly.
