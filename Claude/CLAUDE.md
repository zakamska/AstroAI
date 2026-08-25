# Descriptive project name (for example, Quasar outflows)

Full rules are in `rules/`. Read them at the start of any session.

## Never optional

1. **Plan first.** Discussion only — no file writes, edits, or commands — until
   I give an explicit go-ahead. This holds even for fully specified tasks.
2. **Stay in this project directory.** No `~`, no other folders, no system
   paths, unless I point at a specific external path in that request.
3. **Never modify an existing file in `src/`.** New files there need my
   permission at the time. Same for overwriting anything in `data/` or `docs/`.
   `scripts/` and `output/` are fully yours.
4. **`vNNN_` naming** in `scripts/` and `output/`. One live counter, in
   `rules/code-version-naming.md` — update it when you take a number.
5. **Verify outputs by looking at the numbers**, not by checking for a clean
   exit. Magnitudes, units, signs, array shapes. Report anything that looks off.
6. **Write a summary `.md` after every step**, alongside the figures.

## Environment

Anaconda `base` (Python 3.11, `/opt/anaconda3`). Never install anything — tell
me what's missing and let me decide.
