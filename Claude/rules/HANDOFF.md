# Handoff documents

## When to create one

Create a handoff document when the user asks for one, when a session is about
to end, when context is getting full, or when work needs to be paused and
picked up later (by the user, or a fresh chat with no memory of this one).

## Where it goes

`docs/AGENT_REPORT_<timestamp>.md`. Get the timestamp with
`date +%Y-%m-%d_%H%M%S` — never guess it. Example: output `2026-01-13_144221`
→ filename `AGENT_REPORT_2026-01-13_144221.md`.

## Structure

```markdown
# Agent Report: [Brief Description]

**Date**: YYYY-MM-DD
**Status**: IN_PROGRESS | INTERRUPTED | BLOCKED | COMPLETED
**Reason**: [why this handoff is being created]

## Context In
- Input artifacts used (paths)
- Plan document, if working from one (path)

## Actions
- Files created/modified (exact paths, what was done to each)
- Commands run (exact commands, purpose, outcome)
- Current phase/task and how far along it is

## Evidence
- Test/check outputs, verbatim
- Errors, warnings, partial results
- Pointers to logs/plots/artifacts

## Context Out
- All created/modified artifacts (full paths)
- What's partial vs complete

## Next
- What the next agent (or the user) should do — a single next task if possible
- Blockers and what's needed to clear them, if any
```

## Notes

- This is a single, self-contained document — no task queue, no other agents
  polling for work. It just needs to let a fresh chat pick up where this one
  left off.
- Once written, a handoff document is not edited again — if things change,
  write a new one.
- Follow `rules/NORMS.md` for where files go and `rules/AI-AGENT-CONTRACT.md`
  for what "done" means for any in-progress work being described.
