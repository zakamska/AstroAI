# NORMS: Project layout and definitive vs agent-editable

## Purpose

This document defines how this project directory is organized, and what counts as
definitive (user-provided) vs agent-editable.

Agent behavior and operating modes live in `rules/AI-AGENT-CONTRACT.md`. Handoff
document instructions live in `rules/HANDOFF.md`. Code file naming lives in
`rules/code-version-naming.md`.

---

## Directories

Create any of these on first use.

| Directory   | Purpose                                              | Agent may create new files | Agent may modify/overwrite existing files |
|-------------|-------------------------------------------------------|-----------------------------|--------------------------------------------|
| **data/**   | Data, including relevant internet downloads           | Yes                         | Only if explicitly asked and given permission |
| **docs/**   | Reference docs, instructions, handoffs, including relevant internet downloads | Yes | Only if explicitly asked and given permission |
| **src/**    | User's code — most protected                         | Only with explicit permission | **Never**, under any circumstances |
| **scripts/**| Agent-editable code (.py scripts and notebooks)       | Yes                         | Yes |
| **output/** | Generated figures, tables, results                    | Yes                         | Yes |

There is no separate `downloads/` directory. When saving something fetched from
the internet, decide whether it's data (dataset, catalog, csv, etc. → `data/`) or
reference material (paper, documentation, writeup → `docs/`).

### data/ and docs/

Definitive-ish, but not fully locked. The agent may write **new** files here
(e.g. saving a download, recording a finding, writing a handoff doc into `docs/`).
The agent must **not modify or overwrite an existing file** in either directory
without first asking the user and getting explicit permission for that specific
file.

### src/

The most protected directory. This holds the user's own code.

- The agent may create a **new** file in `src/` only with the user's explicit
  permission, granted at the time.
- The agent must **never modify an existing file in `src/`** — not with
  permission, not as a "quick fix," not ever. If a change to existing `src/`
  code seems necessary, the agent should say so and let the user make the edit,
  or copy the relevant code into `scripts/` to work on there.

### scripts/ and output/

Fully agent-editable. All code (`.py` scripts and notebooks) lives in `scripts/`.
Output from that code (figures, tables, results) goes to `output/`. See
`rules/code-version-naming.md` for naming conventions.

---

## Where the agent works

Day-to-day work happens in `scripts/` and `output/`. The agent may occasionally
write new files into `data/` or `docs/` when appropriate. `src/` is read-only
for the agent except for adding new files with permission.

---

## Rules

All workspace rules live under **`rules/`** at the project root (this file,
`AI-AGENT-CONTRACT.md`, `code-version-naming.md`, `build-verify.md`,
`HANDOFF.md`).

---

## Getting started

To start a new project in this directory: ensure `data/`, `docs/`, `src/`,
`scripts/`, and `output/` exist; create any of them on first use.
