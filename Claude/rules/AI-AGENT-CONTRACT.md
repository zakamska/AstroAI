# AI Agent Contract

## Status

Binding whenever an AI agent works in this project directory. This defines
**how the agent behaves**. Directory layout and file-protection rules live in
`rules/NORMS.md`.

---

## What this is for

- **Reproducible** — someone else can re-run or re-check the work later.
- **Auditable** — evidence exists; nothing important is "in chat only."
- **Low back-and-forth** — the agent can run with well-scoped tasks.
- **Safe** — no silent assumptions; no accidental edits to protected files.
- **Composable across time** — handoffs and restarts don't lose state.

---

## Global invariants

### Plan first, by default

Unless told otherwise, the agent stays in discussion/planning mode: no file
writes, no edits, no command execution — just talking through ideas, options,
and plans. The agent only moves to execution once the user explicitly says so
(e.g. "let's do it," "go ahead," "build it"). This applies even to tasks that
seem fully specified.

### No implicit knowledge

Do not assume conventions, units, environment, or intent. If something is
ambiguous and affects correctness: stop and ask.

### Evidence over explanation

Prefer code inspection, tests, and concrete artifacts (plots, logs, outputs)
over verbal reasoning. Clearly label speculation; minimize it.

### Respect the directory rules

Follow `rules/NORMS.md` exactly: `data/` and `docs/` may receive new files but
not silent overwrites; `src/` may receive new files only with explicit
permission and must never be modified once written, no exceptions.

### Stay in the project directory

All reading and writing happens under this project root. Do not read, write, or
search outside it — no `~`, no other Desktop folders, no system paths — unless
the user points at a specific external path in that request.

The session scratchpad (outside the project, under `/private/tmp/claude-*/`) is
an approved exception, for throwaway working files only. Anything that is part
of the work — code, outputs, notes — belongs in the project.

### Leave the environment alone

Do not install, upgrade, or remove packages, and do not otherwise modify the
Python environment. If something is missing, say what it is, why it is needed,
and which environment it would go into, then let the user decide. When reporting
results, state which interpreter and environment were actually used.

### Determinism when claimed

If something is described as reproducible, state the pinned inputs and
commands used. If randomness is involved, surface the seed/configuration.

---

## Operating modes

State which mode applies if it isn't obvious from the request.

### Mode A — Planning / Discussion (default)

Talking through ideas, options, and plans. No file writes, edits, or command
execution. Ends when the user gives explicit go-ahead to execute.

### Mode B — Working (execute)

Use once the user has given the go-ahead.

- **Scope first**: restate the goal and boundaries; ask if boundaries are
  unclear.
- **Work in stages** for non-trivial tasks — each stage has a goal,
  deliverables, and a stop condition. Don't jump ahead into future stages.
- **Produce concrete artifacts** (files, plots, logs) rather than long
  arguments about what would happen.
- **Write it up**: end each stage with a short summary document alongside the
  outputs — what was done, what the numbers are, what is uncertain, and anything
  found that was not expected, including mistakes and dead ends. Chat is not an
  artifact; a file is.
- **Stop rule**: after delivering the requested artifacts for the current
  stage, stop and wait for confirmation before expanding scope.

### Mode C — Supervising (review & guide)

Use when the user wants review, critique, or diagnosis without changes.

- Read and inspect before hypothesizing.
- Ask clarifying questions early if key constraints are missing.
- Propose a minimal check/test/plot to resolve ambiguity rather than
  debating it.
- End with a clear recommendation and stop — don't start executing unless
  asked.

### Mode D — Handing off

Use when a session is ending, context is filling up, or work needs to be
picked up later (by the user or a fresh chat). See `rules/HANDOFF.md` for the
handoff document format.

---

## End of contract
