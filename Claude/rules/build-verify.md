# Code build: run and verify

When a plan includes building or implementing code in `scripts/` — a `.py`
script or a `.ipynb` notebook — the task is **not complete** until all of the
following are done.

Prefer a plain `.py` script by default: it is usually faster and cheaper to
produce and to run. Use a notebook when the format genuinely helps — interactive
exploration, or when the user wants to see intermediate state cell by cell.

## 1. Execute it

- Script: run it (`python scripts/vNNN_name.py`).
- Notebook: execute it (e.g.
  `jupyter nbconvert --execute --to notebook --inplace <path>` or equivalent).

"Code written" is not "done" without at least one full run.

## 2. Debug until it runs

If execution fails: fix the cause (syntax, imports, paths, APIs, data types),
then re-run. Repeat until it runs top to bottom with no errors.

## 3. Verify outputs

Confirm all outputs promised in the plan actually exist: figures and
tables/files written to the agreed paths (see `rules/code-version-naming.md`
for naming). If the plan included checks or assertions, confirm they pass, or
document why they don't.

Then sanity-check the results themselves — a clean exit is not the same as a
correct answer. Look at the numbers and the figures: are magnitudes, units,
signs, and array shapes plausible? Are there silent NaNs, empty plots, or
all-zero columns? Report anything that looks off rather than passing it along
as verified.

## 4. Definition of done

The user should be able to open the generated output files and inspect them,
and — for a notebook — open it and see executed cells. Only consider the task
complete once that's true, once the outputs have been eyeballed for
plausibility, and once the stage summary has been written (see **Mode B** in
`rules/AI-AGENT-CONTRACT.md`).
