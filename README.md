# ai-tools

A portable reference toolkit for building software well with Claude Code —
in a greenfield project you own or a codebase you've never seen before. It
bundles a spec-driven process, a set of subagents and slash commands, and
framework starting points so a new session lands with the discipline and
tooling already in place. Working a live technical interview or client
scoping session on the clock is one supported mode (see
`docs/interview-cheat-sheet.md`), not the whole of it.

## What's in here

- **`MANIFEST.md`** — the routable catalog for any AI agent: every asset, its
  trigger, and how to invoke it, plus the precedence rule and working flow.
  The fastest way for another agent — or another tool that doesn't auto-load
  `CLAUDE.md` — to understand and act on the whole toolkit.
- **`CLAUDE.md` + `AGENTS.md`** — the always-on operating context. `AGENTS.md`
  holds the nine rules (clarify, validate trust boundaries, match conventions,
  scope discipline, verify, narrate, plan-the-whole-feature-set, test at the
  right level, atomic-commits/feature-branch) in a tool-neutral form other
  agents (Cursor, etc.) also read; `CLAUDE.md` imports it and adds the
  Claude-Code toolkit pointers. Unlike the constitution and cheat sheet, these
  load into every turn's context automatically.
- **`.claude/agents/`** — read-only subagents to delegate to:
  - **`codebase-scout`** — reconnaissance. Spin it off in the background to map
    an unfamiliar repo (architecture, relevant files, existing patterns, open
    questions) while you keep talking through requirements in the foreground.
  - **`verifier`** — drives a change end-to-end (runs the suite, exercises the
    golden path + an edge case, builds/typechecks) and reports pass/fail.
  - **`diff-reviewer`** — reads back the current diff for scope creep,
    convention mismatch, likely bugs, and leftovers before you commit.

  None of them edit, commit, or install — they report back; you stay in
  control of what actually changes.
- **`.claude/commands/`** — the loop as one-keystroke slash commands:
  `/orient`, `/mini-spec`, `/verify`, `/commit`, `/pr`.
- **`.claude/settings.json`** — a read-only permission allowlist (git recon,
  test/build/typecheck runners) so recon doesn't stop for a prompt on every
  command, plus `defaultMode: acceptEdits` so edits auto-apply.
- **`stacks/`** — repo-local, framework-specific convention packs to read
  before writing code in a given stack (starting with `stacks/nextjs.md`).
  They travel with the repo, unlike session-level plugin skills.
- **`.specify/`** — the Spec-Kit engine (generic spec/plan/tasks/
  checklist templates + the PowerShell automation scripts that back
  them), copied over from a real, working Spec-Kit project rather than
  reinstalled from scratch.
- **`.claude/skills/speckit-*`** — the actual slash-command skills that
  drive Spec-Kit (`specify`, `plan`, `tasks`, `clarify`, `analyze`,
  `checklist`, `implement`, `constitution`, `converge`, `taskstoissues`).
- **`.specify/memory/constitution.md`** — the general engineering-process
  constitution the toolkit is built around: nine principles (clarify before
  building, validated trust boundaries, match conventions, scope discipline,
  verify, narrate, plan the whole feature set first, test at the right level,
  atomic commits on a feature branch). Applies to greenfield-owned and
  unfamiliar codebases alike; the time-boxed/live mode is documented as one
  explicit exception in its Governance section.
- **`docs/interview-cheat-sheet.md`** — the same discipline, condensed
  to one page you can actually glance at mid-conversation.

## Bootstrapping on a new machine

Clone this repo, then copy the toolkit files straight into the root of
whatever repo you're actually working in — they're meant to travel with
the session, not live in `~/.claude/` (the target codebase has nothing
to do with this one).

**bash:**
```bash
git clone https://github.com/jonupchurch/ai-tools.git /tmp/ai-tools
cp -r /tmp/ai-tools/.claude /tmp/ai-tools/.specify /tmp/ai-tools/stacks \
      /tmp/ai-tools/CLAUDE.md /tmp/ai-tools/AGENTS.md /tmp/ai-tools/MANIFEST.md \
      /path/to/target-repo/
```

**PowerShell:**
```powershell
git clone https://github.com/jonupchurch/ai-tools.git C:\temp\ai-tools
Copy-Item -Recurse C:\temp\ai-tools\.claude, C:\temp\ai-tools\.specify, C:\temp\ai-tools\stacks, C:\temp\ai-tools\CLAUDE.md, C:\temp\ai-tools\AGENTS.md, C:\temp\ai-tools\MANIFEST.md -Destination C:\path\to\target-repo\
```

> **If the target repo already has a `.claude/settings.json`, `CLAUDE.md`, or
> `AGENTS.md`**, copying will overwrite it — merge by hand instead of clobbering
> the repo's own. And if you're dropping this into a repo you don't own, drop
> `defaultMode: acceptEdits` from `settings.json` (or move it to a gitignored
> `.claude/settings.local.json`) so you don't hand teammates silent auto-accept.

Then start (or restart) your Claude Code session inside the target
repo — `codebase-scout` and the `speckit-*` skills are now available
there. None of this needs to be committed to the target repo's own
history unless you want it to be.

If you don't have network access to clone on the new machine, the
fallback is having this repo's raw file contents open somewhere you
can copy-paste from (a browser tab on the GitHub page works) and
recreating the handful of files by hand — it's a small enough set that
this is realistic under time pressure.

## Using it

1. Read `docs/interview-cheat-sheet.md` once before you start. (The same
   rules load automatically via `CLAUDE.md`/`AGENTS.md`.)
2. Run `/orient` to get a fast read on the repo and kick `codebase-scout`
   off in the background — investigation you don't need to watch live.
3. Walk the loop, using the commands where they help: `/mini-spec` to lock
   scope → lightweight plan → build to match existing conventions (read the
   relevant `stacks/` pack first) → `/verify` → `/commit` / `/pr`. Narrate
   throughout.
4. If there's real time to work with, the full `speckit-specify` →
   `speckit-plan` → `speckit-tasks` → `speckit-implement` chain is
   available too — but the cheat sheet and constitution are the
   realistic path when there isn't.

## Extending this

- **Subagents** — add more to `.claude/agents/`, same frontmatter format as
  `codebase-scout.md` (`name`, `description`, `tools`, `model`, then the
  system-prompt body).
- **Slash commands** — add more to `.claude/commands/`; each is a Markdown
  file with optional frontmatter (`description`, `argument-hint`,
  `allowed-tools`) and `$ARGUMENTS` in the body.
- **Stack packs** — add `stacks/<framework>.md` following the shape of
  `stacks/nextjs.md` (defaults → where things go → don't → verify).
