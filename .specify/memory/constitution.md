<!--
Sync Impact Report
==================
Version: 1.0.0 (initial)
Status: Reference constitution for time-boxed work in an unfamiliar
  codebase (interview/client-simulation scenarios) — adapted from the
  playm8z project's own ratified constitution (D:\Codelib\playm8z\
  .specify\memory\constitution.md, v1.0.0), which itself traced its
  process shape back through InterruptVector and PrintingSite. Per that
  same lineage rule: this is a starting point for PROCESS, not product
  content — playm8z's actual tech stack, ADRs, and feature scope are
  NOT carried over, since this repo is meant to travel into a different
  codebase every time it's used.
Adapted for: being dropped into someone else's existing, unfamiliar
  codebase under real time pressure, often narrating decisions live to
  a client/interviewer rather than writing them into commit history
  after the fact.
-->

# Reference Constitution — Working Fast in an Unfamiliar Codebase

## Core Principles

### I. Clarify Before Building (NON-NEGOTIABLE)

Before writing code, capture — even in a few bullet points, even just
said out loud — what's actually being asked: the user story, the
acceptance criteria, and what's explicitly OUT of scope. If a
requirement is ambiguous, ask the client/interviewer rather than
silently picking an assumption; if asking isn't possible in the moment,
state the assumption out loud before proceeding so it can be corrected
early rather than discovered at the end.

Rationale: the single most common way a technically-correct solution
still fails an interview or a client engagement is solving the wrong
problem confidently. A five-second clarifying question is always
cheaper than a wrong solution.

### II. Validated Trust Boundaries

Anything crossing a trust boundary — form input, API request bodies,
query params, anything a user or another system controls — gets
validated before use, following whatever validation convention the
existing codebase already has (Zod, a schema library, manual checks —
match what's there, don't introduce a new one for one change). Never
trust client-side state for authorization; check it server-side, the
way the codebase already does.

Rationale: this is universal, not project-specific — the codebase's
own existing pattern is usually right there to follow, so this
principle is about finding and matching it, not inventing a new one.

### III. Match Existing Conventions

This is someone else's codebase. Its existing design system, code
style, file layout, and UX patterns are the source of truth — not
personal preference. Before writing new code in an unfamiliar area,
find the nearest existing analog (a similar component, a similar
route, a similar test) and follow its shape. Deviating is sometimes
right, but say why out loud when it happens.

Rationale: consistency with what's already there reads as competence
fast; a stylistically-foreign addition reads as not having actually
understood the codebase, even if the logic is correct.

### IV. Scope Discipline (NON-NEGOTIABLE)

Ship the smallest complete slice that actually satisfies what was
asked. Resist gold-plating or solving adjacent problems nobody raised.
If a good idea surfaces mid-build that's outside the current ask,
name it out loud ("worth doing, but separate from this") rather than
silently expanding scope.

Rationale: under time pressure, scope creep is the fastest way to run
out of time with nothing finished; a client/interviewer trusts a
narrow, complete answer more than a broad, half-built one.

### V. Verify Before Calling It Done

Before saying "done," actually check it: run the existing test suite
if one exists, manually exercise the golden path (and the one most
obvious edge case), and read back your own diff once. If the codebase
has a build/typecheck step, run it. "I believe this works" and "I
checked this works" are different claims — say which one you're
making.

Rationale: a demo that breaks the moment someone else touches it is
worse than admitting a rough edge honestly; verification is cheap
insurance against exactly that.

### VI. Narrate the Reasoning

In a live setting there's no commit history to reconstruct intent from
later — the narration IS the record. Say what you're about to do and
why before doing it, especially for a non-obvious call (why this
approach over an alternative, why this scope boundary, what you're
explicitly deferring). Keep a short running list of decisions/
assumptions so you can answer "why did you do X" without having to
reconstruct it from memory under pressure.

Rationale: the reasoning is frequently what's actually being evaluated,
not just the final diff — make it visible as you go, don't save it for
a retrospective explanation.

## Workflow for a Brand-New, Unfamiliar Codebase

1. **Fast orientation pass** (minutes, not hours) — stack, entry
   points, directory conventions, how a request flows, and the handful
   of existing patterns you'll need to match. Delegate this to a
   background investigation agent (see `.claude/agents/codebase-scout.md`
   in this repo) so it runs while you're still talking through
   requirements — don't block the client conversation on your own
   manual exploration.
2. **Clarify scope with the client** — the actual ask, acceptance
   criteria, explicit non-goals (Principle I). A 3-bullet mini-spec is
   enough; don't over-invest in ceremony you don't have time for.
3. **Lightweight plan** — the approach, the files/areas it touches, and
   the one or two real tradeoffs, said out loud or jotted down. Skip
   this only for genuinely trivial changes.
4. **Implement, matching existing conventions** (Principle III).
5. **Verify** (Principle V) before presenting it as finished.
6. **Narrate throughout** (Principle VI) — don't save the explanation
   for the end.

## Governance

This is a reference starting point, not inherited authority — the
actual codebase's own conventions win whenever they conflict with
anything here. Adapt freely per engagement; there's no ratification
ceremony required for a working reference document like this one.

**Version**: 1.0.0 | **Ratified**: 2026-07-15 | **Last Amended**: 2026-07-15
