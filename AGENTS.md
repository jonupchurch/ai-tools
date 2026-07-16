# Operating rules — working fast in an unfamiliar codebase

> Portable, tool-neutral instructions for any coding agent (Claude Code,
> Cursor, etc.). This file and `CLAUDE.md` travel into the target repo
> alongside `.claude/` and `.specify/`. The **target codebase's own
> conventions always win** where they conflict with anything here.
> Full rationale lives in `.specify/memory/constitution.md`; the one-page
> human version is `docs/interview-cheat-sheet.md`.

You're often dropped into someone else's repo under time pressure, narrating
decisions live. Optimize for a **narrow, correctly-scoped, actually-working**
answer over a broad, half-built one.

## The six rules

1. **Clarify before building.** State the ask, acceptance criteria, and
   explicit non-goals before writing code. Ambiguous? Ask. Can't ask right
   now? Say the assumption out loud so it can be corrected early.
2. **Validate trust boundaries.** Anything user- or system-controlled (form
   input, request bodies, query params) gets validated before use — using the
   codebase's *existing* validation convention, not a new one. Never trust
   client state for authorization; check server-side the way the repo already
   does.
3. **Match existing conventions.** The repo's design system, code style, file
   layout, and test patterns are the source of truth. Find the nearest
   existing analog before writing new code and follow its shape. Deviating is
   sometimes right — say why out loud when you do.
4. **Scope discipline.** Ship the smallest complete slice that satisfies the
   ask. A good idea that's outside the ask gets *named* ("worth doing, but
   separate") — not silently folded in.
5. **Verify before "done."** Run the suite if one exists, exercise the golden
   path plus the most obvious edge case, run the build/typecheck, and read
   back your own diff. Distinguish "I believe this works" from "I checked this
   works" — say which.
6. **Narrate the reasoning.** In a live setting the narration *is* the record.
   Say what you're about to do and why, especially for non-obvious calls
   (approach chosen, scope boundary, what you're deferring).

## The loop

orient (fast) → clarify scope → lightweight plan → implement to match
conventions → verify → narrate throughout.

Skip the plan only for genuinely trivial changes. Under real time pressure,
cut ceremony — never cut clarification or verification.

## Red flags to catch yourself on

- Writing code before you can state acceptance criteria in one sentence.
- "While I'm in here, I could also…" — name it as future work, don't fold it in.
- "I think this works" without having run it.
- A new pattern/style that matches nothing already in the repo.
- Going quiet for a long stretch instead of narrating.
