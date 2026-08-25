# Using AstroAI with Claude Code: A Recommended Work Process

This repository provides minimal infrastructure for using
[Claude Code](https://claude.com/claude-code) in a scientifically productive way.
The files under [`rules/`](./rules/) define how the agent behaves in your
project, and a small settings file defines what it is permitted to do at all.

The sections below explain how to get started, how I work day to day, and how
the setup addresses the things I was worried about before I let an AI agent
anywhere near my research.

---

## Table of contents

1. [Concerns and how this setup helps](#concerns-and-how-this-setup-helps)
2. [First-time setup](#first-time-setup)
3. [Day-to-day workflow](#day-to-day-workflow)
4. [Versioned scripts and outputs](#versioned-scripts-and-outputs)
5. [Planning before doing](#planning-before-doing)
6. [Handoffs and long sessions](#handoffs-and-long-sessions)

---

## Concerns and how this setup helps

Before adopting an AI coding agent for research, I cared about several things:

| Concern | What I wanted |
|---|---|
| **Safety of my computer** | The agent must stay inside my project. It must not wander into my home directory, my other projects, or system files. |
| **Safety of my code and data** | My own code and data must not be silently modified or overwritten. |
| **Hallucinations** | No invented citations, numbers, or "facts." |
| **Sycophantic fluff** | Clear reasoning and honest disagreement, not flattery. |
| **Trust** | When the agent tells me something, I need a way to check it. |

No setup removes these risks entirely. The rules in `Claude` reduce them substantially. 

- **Workspace confinement** — All agent activity stays inside the project directory you open as the workspace (no edits under your home folder, `/tmp`, etc.). Claude Code keeps per-project metadata outside your workspace, under `~/.claude/` — principally the full transcript of every session `~/.claude/projects/<project>/<session-id>.jsonl`, plus a `memory/` directory for durable notes it's asked to remember, along with global settings, shell snapshots, session state, backups and telemetry — and a session-scoped scratch area under `/private/tmp/claude-*/` for throwaway working files and background-command logs, which is cleared per session.
- **Directory layout ([NORMS](./rules/NORMS.md))** — User-provided material lives in `data/`, `src/`, and `docs/`; the agent's code and generated results go in `scripts/` and `output/`, with clear rules about not overwriting your files without permission.
- **Agent contract ([AI-AGENT-CONTRACT](./rules/AI-AGENT-CONTRACT.md))** — Handoffs, evidence, and artifact state (what is authoritative vs in progress).
- **Notebook discipline** — Agent runs and verifies notebooks before calling work done; version numbers increase sequentially for each step so you can trace what produced what.

**Trust is earned, not assumed.** Treat the agent as a fast assistant that must show its work: cite files it read, run code when possible, and record results in the project (not only in chat). Your `src/project_description.txt` (below) and versioned outputs are part of that audit trail.

---

## First-time setup

This workflow starts conservative. You can relax it later.

### 1. Create a home for AI-assisted work

Choose one directory per project:

```text
/Users/YourName/AI/new_project/
```

### 2. Copy the files provided in this GitHub repository

```text
/Users/YourName/AI/new_project/CLAUDE.md
	tells Claude the main project rules and where to find them, read automatically
/Users/YourName/AI/new_project/rules/
    AI-AGENT-CONTRACT.md      how the agent behaves; operating modes
    NORMS.md                  directory layout and what is protected
    code-version-naming.md    vNNN_ naming and the live version counter
    build-verify.md           run it, debug it, verify the outputs
    HANDOFF.md                handoff document format
/Users/YourName/AI/new_project/.claude/settings.json
	enforced project settings: what is Claude allowed to read, write, execute
```

Optionally create `data/`, `docs/`, `src/`, `scripts/`, and `output/` — they can
also be created on first use.

### 3. Start Claude Code in the project directory

On the desktop agent, click New (left menu), then above the prompt field click on the folder button with a plus sign on it and select your directory. This will launch an agent inside `/Users/YourName/AI/new_project`. That directory becomes the working directory, and everything in this setup is written relative to it.

### 4. Sanity-check

```text
Can you please take a look in the rules folder and tell me what the main rules
for working here are? Can you tell me what you are allowed to do with files without permission? 
```

You should hear back about confinement, plan-first as the default mode, the directory
permission table, the `vNNN_` naming scheme, and the requirement to run and
verify code before calling it done. It should also tell you that the json file does not allow `rm`, `pip install`, `conda install` and `conda remove`. 

Pay attention to anything that's *missing*. If it says it has some inconsistency in the rules, ask it what it is and how it can fix it. If anything is unclear, ask for clarifications or how it would handle it if you asked it do perform a certain task. 

**The way to set up your agent is to talk to it and ask it to do certain things in a certain way.** 

---

## Day-to-day workflow

What works well for me is to **write first, then prompt**.

### 1. Maintain a project description

Create a plain-text file and put it in `src/`, so that it is protected:

```text
/Users/YourName/AI/new_project/src/project_description.txt
```

Put **everything** important in there before you ask for much:

- Big-picture goals and constraints
- What each dataset is, where it came from, and what state it is in
- References — papers, URLs, naming conventions
- What you have already tried, including what failed and why
- What each analysis version was meant to do

This file is for you as much as for the agent. Mine ran to several pages and
carried the whole project. It is worth more than any prompt you will write.

### 2. Start by asking what is missing

```text
Please read src/project_description.txt.
Don't try to plan the whole project at once. First tell me what information is
missing so that I can start pulling it in. Then let's prioritise one doable and
useful task and accomplish it.
```

Asking what is *missing* before asking for work surfaces the gaps in your own
material early, when they are cheapest to fix. In my case the survey turned up
data referenced but absent, atomic data that existed only as PNG images, a line
list that lived only as hard-coded arrays scattered across eight cells of a
9 MB notebook, and no observation log at all. That shaped everything after it.

### 3. Work in small steps

Give one focused task at a time. Refine the description as you go. Move to
implementation only when you are happy with the approach.

### 4. Written summary after every step

I found myself repeatedly asking,

```text
After every step, please also write what you just told me as a .md file.
```

Chat scrolls away and cannot be cited. A summary sitting in `output/` next to
the figures can. By the end of my project the summaries *were* the deliverable:
they held the results, the caveats, the bugs found, the reasoning, and the open
questions, in a form I could use in a draft. 

The .md summaries are required by the rules now. I asked an AI agent for help in installing a Chrome App that reads .md files. 

---

## Versioned scripts and outputs

The [code-version-naming](./rules/code-version-naming.md) rule requires new code
in `scripts/` to carry a three-digit prefix:

```text
v000_line_measurements.py
v001_flux_calibration.py
```

Anything written to `output/` uses the **same** prefix:

```text
v000_line_measurements.py   ->   output/v000/v000_line_fits.png
                                 output/v000/v000_measurements.ecsv
```

The version number belongs to the **task**, not the file — one number can cover a
script, a helper, and all of their outputs. Keep one live counter in the rule
file and update it as numbers are taken.

**On notebooks:** I used to insist on them, but not anymore. Plain `.py` scripts are usually faster
to produce, faster to run, and much easier to re-run and diff. Use a notebook
when the format genuinely helps — interactive exploration, or when you want to
see intermediate state cell by cell. 

---

## Planning before doing

The [contract](./rules/AI-AGENT-CONTRACT.md) makes planning the default. Unless I
say otherwise, the agent stays in discussion mode — talking through options, no
file writes, no edits, no commands — and moves to execution only when I say so
explicitly ("go ahead", "let's do it", "build it"). This holds even for tasks
that look completely specified.

I spend most of my time in that mode. The contract also defines a supervising
mode for when I want review or diagnosis with no changes at all, and a handoff
mode for ending a session.

The friction point is not getting the agent to plan — it is getting it to **stop**
after delivering rather than rolling on into the next thing. The contract has an
explicit stop rule for this: after delivering the artifacts for the current
stage, stop and wait.

---

## Handoffs and long sessions

When I see the context circle filling up (the circle in the bottom right corner of the desktop agent), just in case I do this: 

```text
Please write a handoff document.
```

The format is in [HANDOFF](./rules/HANDOFF.md); the file goes to
`docs/AGENT_REPORT_<timestamp>.md` with the timestamp taken from `date`, never
guessed. It records what was done, what evidence exists, what is incomplete, and
what to do next.

---

## Summary

1. **One folder per project**, with `rules/`, `CLAUDE.md`, and
   `.claude/settings.json` inside it, and Claude Code started there.
2. **`CLAUDE.md` is read automatically; `rules/` is not.** Put the
   non-negotiables in the former, the detail in the latter.
3. **Permissions are enforced; rules are followed.** Use permissions for anything
   that must never happen, rules for everything else.
4. **`src/project_description.txt`** holds intent, references, and version notes.
5. **Plan first**, explicit go-ahead, and stop after delivering.
6. **`vNNN_` naming** ties code in `scripts/` to results in `output/`.
7. **Verify outputs by looking at the numbers**, not by checking for a clean exit.
8. **A summary `.md` after every step.** Chat scrolls away; files do not.

Enjoy — and adjust the workflow to match how conservative you want to be.
