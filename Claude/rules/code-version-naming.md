# Code version naming (scripts/ and output/ only)

Applies only to files the agent creates in **`scripts/`** and **`output/`**.
Does not apply to `src/`, `data/`, `docs/`, or existing files.

## Naming

Every new code file in `scripts/` — `.py` script or `.ipynb` notebook — is
named:

```
vNNN_descriptive_name.py
vNNN_descriptive_name.ipynb
```

`NNN` is a three-digit version number (`v000`, `v001`, ...). The `descriptive_name`
should be a short, clear label for what the file does — the agent should feel
free to suggest one.

## The version number belongs to the task, not the file

A single version number can cover **multiple files** when they're different
parts of the same piece of work. For example, if one bigger task needs both a
figure-making script and a separate notebook for additional calculations, both
get the same version number with different descriptive names:

```
v015_make_figures.py
v015_additional_calculations.ipynb
```

There is **no requirement of one file per version number.** A new version
number is used when starting a new task, not automatically for every new file.

## Outputs in output/

Files written to `output/` (plots, tables, summaries, FITS, ECSV, etc.) use
the **same** `vNNN_` prefix as the code that produced them, so it's clear
which output came from which task:

```
v015_make_figures.py           →  output/v015_summary_plot.png
v015_additional_calculations.ipynb  →  output/v015_extra_table.csv
```

## Sequencing

Versioning is sequential across **all** code in `scripts/`, regardless of
file type — `.py` and `.ipynb` share one counter. The line below is the live
counter: update it each time a version number is taken.

**Next version: v000**
