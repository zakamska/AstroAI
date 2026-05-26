# NORMS: Project layout and definitive vs agent-editable

## Purpose

This document defines:
- **how the project directory is organized**, and
- **what counts as definitive (user-provided) vs agent-editable**.

Detailed agent execution mechanics (modes, handoffs, evidence) live in `.cursor/rules/AI-AGENT-CONTRACT.md`.

---

## Scope

- **All work lives in the project (workspace) root.** No git repos or multi-project structure are assumed.
- Path and directory constraints in `.cursor/rules/workspace-confinement.mdc` also apply (paths under workspace only; use the standard directories below).

---

## Directories

The project uses six directories. Create any of them on first use.

| Directory      | Purpose                          | Agent may create | Agent may overwrite existing |
|----------------|----------------------------------|------------------|------------------------------|
| **downloads/** | Internet downloads               | Yes              | No (unless user explicitly asks) |
| **data/**      | User-provided data               | Yes              | No (unless user explicitly asks) |
| **src/**       | User-provided code               | Yes              | No (unless user explicitly asks) |
| **docs/**      | User-provided instructions, docs| Yes              | No (unless user explicitly asks) |
| **scripts/**   | Agent-editable code (Python, Jupyter, Marimo) | Yes | Yes |
| **output/**    | Generated figures, tables, results | Yes            | Yes |

- **Definitive directories** — `downloads/`, `data/`, `src/`, `docs/`. Files here are treated as authoritative. The agent may write **new** files when appropriate (e.g. saving a download, recording a finding) but must **not overwrite** existing files unless the user explicitly requests it (e.g. “replace data/foo.csv”).
- **Agent-editable directories** — `scripts/`, `output/`. All code (Python, Jupyter notebooks, Marimo) lives in `scripts/`. Output from these scripts (figures, tables) goes to `output/`. The agent may create and modify files in both.

---

## Where the agent works

- **Primary work** is in **scripts/** and **output/**: editing and running code, saving figures and tables.
- The agent may occasionally write **new** files into `downloads/`, `data/`, `src/`, or `docs/` when appropriate, but must not overwrite existing files in those directories unless the user explicitly requests it.

---

## Rules and commands

- All workspace rules live under **`.cursor/rules/`** (e.g. this file, AI-AGENT-CONTRACT, workspace-confinement).
- All workspace commands live under **`.cursor/commands/`**.
- This project uses `.cursor/` as the single source for rules and commands.

---

## Getting started

To start a new project in this directory: ensure the six directories exist; create any of them on first use.
