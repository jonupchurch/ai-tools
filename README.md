# ai-tools

A portable reference toolkit for working fast, with Claude Code, in a
codebase you've never seen before — built for scenarios like a live
technical interview: dropped into an unfamiliar repo, working with a
client/interviewer to scope and ship a solution, on the clock.

## What's in here

- **`.claude/agents/codebase-scout.md`** — a read-only reconnaissance
  subagent. Spin it off in the background to investigate the codebase
  (architecture, relevant files, existing patterns, open questions)
  while you keep talking through requirements in the foreground. It
  never edits, writes, commits, or installs anything — it reports back,
  you stay in control of what actually changes.
- **`.specify/`** — the Spec-Kit engine (generic spec/plan/tasks/
  checklist templates + the PowerShell automation scripts that back
  them), copied over from a real, working Spec-Kit project rather than
  reinstalled from scratch.
- **`.claude/skills/speckit-*`** — the actual slash-command skills that
  drive Spec-Kit (`specify`, `plan`, `tasks`, `clarify`, `analyze`,
  `checklist`, `implement`, `constitution`, `converge`, `taskstoissues`).
- **`.specify/memory/constitution.md`** — a generic constitution
  distilling the process discipline this toolkit is built around
  (clarify before building, match existing conventions, scope
  discipline, verify before calling it done, narrate the reasoning),
  reframed for "someone else's existing codebase, under time pressure"
  rather than a greenfield build.
- **`docs/interview-cheat-sheet.md`** — the same discipline, condensed
  to one page you can actually glance at mid-conversation.

## Bootstrapping on a new machine

Clone this repo, then copy `.claude/` and `.specify/` straight into the
root of whatever repo you're actually working in — they're meant to
travel with the session, not live in `~/.claude/` (the target codebase
has nothing to do with this one).

**bash:**
```bash
git clone https://github.com/jonupchurch/ai-tools.git /tmp/ai-tools
cp -r /tmp/ai-tools/.claude /tmp/ai-tools/.specify /path/to/target-repo/
```

**PowerShell:**
```powershell
git clone https://github.com/jonupchurch/ai-tools.git C:\temp\ai-tools
Copy-Item -Recurse C:\temp\ai-tools\.claude, C:\temp\ai-tools\.specify -Destination C:\path\to\target-repo\
```

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

1. Read `docs/interview-cheat-sheet.md` once before you start.
2. Ask Claude to spin off `codebase-scout` in the background for any
   investigation you don't need to watch happen live.
3. Follow the constitution's workflow section: orient → clarify →
   lightweight plan → build to match existing conventions → verify →
   narrate throughout.
4. If there's real time to work with, the full `speckit-specify` →
   `speckit-plan` → `speckit-tasks` → `speckit-implement` chain is
   available too — but the cheat sheet and constitution are the
   realistic path when there isn't.

## Extending this

Add more subagents to `.claude/agents/` as you find you want them —
same frontmatter format as `codebase-scout.md` (`name`, `description`,
`tools`, `model`, then the system prompt body).
