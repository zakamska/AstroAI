# Using AstroAI with Cursor: A Recommended Work Process

This repository provides minimal infrastructure for using [Cursor](https://cursor.com) in a scientifically productive way. The files under [`.cursor/`](./.cursor/) define **rules** (always-on constraints for the agent) and **commands** (repeatable workflows you invoke with `/command-name`). Together they shape how the agent behaves in your project.

The sections below explain how to get started, how I work day to day, and how the setup addresses common concerns about safety, privacy, and trust.

---

## Table of contents

1. [Concerns and how this setup helps](#concerns-and-how-this-setup-helps)
2. [First-time setup](#first-time-setup)
3. [Day-to-day workflow](#day-to-day-workflow)
4. [Versioned notebooks and outputs](#versioned-notebooks-and-outputs)
5. [Plan mode vs Agent mode](#plan-mode-vs-agent-mode)
6. [Handoffs when context runs long](#handoffs-when-context-runs-long)

---

## Concerns and how this setup helps

Before adopting an AI coding agent for research, I cared about several things:

| Concern | What I wanted |
|--------|----------------|
| **Safety of my computer** | The agent must not edit files I did not intend it to touch. |
| **Safety of my information** | Sensitive material must not end up where I do not want it (e.g. casually uploaded or scattered outside the project). |
| **Hallucinations** | I do not want invented citations, numbers, or “facts.” |
| **Sycophantic fluff** | I do not want flattering or evasive answers instead of clear reasoning. |
| **Trust** | When the agent states something, I need a path to verify it (evidence, reproducibility, explicit uncertainty). |

No setup eliminates these risks entirely. The rules and commands in `.cursor/` address many of them **in part or in full**, especially when combined with a conservative workflow:

- **Workspace confinement** — All agent activity stays inside the project directory you open as the workspace (no edits under your home folder, `/tmp`, etc.).
- **Directory layout ([NORMS](./.cursor/rules/NORMS.md))** — User-provided material lives in `data/`, `src/`, and `docs/`; the agent’s code and generated results go in `scripts/` and `output/`, with clear rules about not overwriting your files without permission.
- **Agent contract ([AI-AGENT-CONTRACT](./.cursor/rules/AI-AGENT-CONTRACT.md))** — Handoffs, evidence, and artifact state (what is authoritative vs in progress).
- **Notebook discipline** — Agent runs and verifies notebooks before calling work “done”; version numbers increase sequentially for each step so you can trace what produced what.

**Trust is earned, not assumed.** Treat the agent as a fast assistant that must show its work: cite files it read, run code when possible, and record results in the project (not only in chat). Your `project_description.txt` (below) and versioned outputs are part of that audit trail.

---

## First-time setup

This workflow starts conservative; you can relax or extend it later.

### 1. Create a home for AI-assisted work

Choose a convenient location for all projects. For example:

```text
/Users/YourName/AI/
```

Inside it, create a directory for one project:

```text
/Users/YourName/AI/new_project/
```

### 2. Add the `.cursor` infrastructure

Copy the entire `.cursor` directory from this repository into `new_project/`:

```text
/Users/YourName/AI/new_project/.cursor/
```

(Include `rules/` and `commands/` and their files.)

Optionally create the standard project folders described in [NORMS](./.cursor/rules/NORMS.md) — they can also be created on first use:

- `downloads/`, `data/`, `src/`, `docs/` — your inputs and documentation  
- `scripts/`, `output/` — agent-editable code and generated results  

### 3. Open the folder as a Cursor workspace

1. Start Cursor.  
2. Use **Open Workspace** (or **File → Open Folder**).  
3. In the file picker, select `/Users/YourName/AI/new_project`.

Everything the agent does should be confined to that folder.

### 4. Sanity-check the rules

In the agent prompt, try:

```text
Hello. Can you please tell me what the important rules of this workspace are?
```

You should see answers along these lines:

- Research should be **reproducible** and **auditable** (evidence, handoffs, clear artifact state).  
- **All agent activity is confined to the current workspace** (the project directory you opened).  
- Distinction between **your** directories (`data/`, `src/`, `docs/`) and **agent** directories (`scripts/`, `output/`).

If that matches what you see, you are ready to proceed.

---

## Day-to-day workflow

What works well for me is to **write first, then prompt**.

### 1. Maintain a project description file

Create a plain-text file in the project root:

```text
/Users/YourName/AI/new_project/project_description.txt
```

Put **everything** important in there before you ask the agent to build much:

- Big-picture goals and constraints  
- References (papers, URLs, naming conventions)  
- Pointers to data or code (or drop files into `data/`, `src/`, `docs/` as needed)  
- Notes on what each analysis version should do (see [versioning](#versioned-notebooks-and-outputs))

This file is for **you** and for the **agent**. I update it as the project evolves.

### 2. Start tasks by pointing the agent at that file

After the description is in good shape, open a session with something like:

```text
Please read project_description.txt and take a look at all the files in the workspace.
Our first task is: [describe the first concrete task here].
```

Then give a **specific** first task (one focused step works better than a vague “do the whole project”).

### 3. Iterate in small steps

Refine `project_description.txt`, adjust plans, and only move to implementation when you are satisfied with the approach (see [Plan mode](#plan-mode-vs-agent-mode)).

---

## Versioned notebooks and outputs

Rules in [notebook-version-naming](./.cursor/rules/notebook-version-naming.mdc) require new notebooks in `scripts/` to be named with a three-digit prefix, e.g.:

- `v000_exploratory_plots.ipynb`  
- `v001_catalog_crossmatch.ipynb`  

Anything that notebook writes to `output/` should use the **same** prefix (e.g. `v000_summary.md`, `v001_candidates.fits`).

That keeps `scripts/` and `output/` easy to scan and ties each deliverable to one notebook. I record in `project_description.txt` what each version was meant to do and what we learned.

When you create `v000`, update the “next version” line in that rule file to `v001`, and so on.

---

## Plan mode vs Agent mode

I keep Cursor in **Plan** mode about **95%** of the time. I switch to **Agent** mode only when I am happy with the plan and want the agent to implement, run, and debug code (especially notebooks).

| Mode | Typical use |
|------|----------------|
| **Plan** | Discuss approach, refine requirements, update the plan — **no** (or minimal) file writes and execution. |
| **Agent** | Edit files, run terminals, execute and fix notebooks. |

**How to switch:** In the prompt area, click the small control on the left (often shown as a mode selector or “+”) and choose **Plan** or **Agent**. The default is usually **Agent**.

For notebook work, [notebook-build-verify](./.cursor/rules/notebook-build-verify.mdc) rule says that the agent is supposed to do a full execute-and-verify cycle once you are in Agent mode and have approved the plan. However, sometimes this fails and you need to remind the agent of the rule. 

---

## Handoffs when context runs long

Long sessions fill the context window. Before starting fresh (or when you want a clean checkpoint), use the **`/create-handoff`** command (defined in [`.cursor/commands/create-handoff.md`](./.cursor/commands/create-handoff.md)).

That command instructs the agent to write a structured report under `docs/`, typically:

```text
docs/AGENT_REPORT_YYYY-MM-DD_HHMMSS.md
```

The report summarizes what was done, what evidence exists, what is incomplete, and what to do next — so a new chat (or a future you) can continue without relying on chat memory alone.

A shorter alias exists: **`/bye`** runs handoff with sensible defaults.

---

## Summary

1. **One folder per project**, with `.cursor` inside it, opened as the Cursor workspace.  
2. **`project_description.txt`** holds intent, references, and version notes.  
3. **Plan first**, then **Agent** to implement and verify.  
4. **`vNNN_` naming** links notebooks in `scripts/` to outputs in `output/`.  
5. **`/create-handoff`** preserves state when context is tight.

Enjoy — and adjust the workflow to match how conservative you want to be.
