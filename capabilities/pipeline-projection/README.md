# Pipeline Projection

## What it is

A cohort model for testing a timing claim: when a hiring shock reduces the
number of juniors entering a pipeline, how long before the resulting gap
shows up at the next level up, and does a later hiring recovery arrive in
time to prevent it? The core move: track each hired cohort forward by a
training lag N, apply an ongoing attrition rate to the resulting stock, and
compare the stock each scenario produces against what a baseline (no shock)
path would have produced.

The build (`spec.md`, `model.xlsx`) follows this repo's standard shape:
named-range inputs, a per-scenario cohort projection, and two named
training-lag cases (primary and sensitivity) run side by side so a claim's
timing can be checked against both at once rather than committed to a single
assumption.

## Where it was exercised

- **economic-research** — testing H2 of the research brief's timing
  hypothesis: does a mid-level talent shortage emerge under depressed
  junior hiring, and does a later recovery in hiring arrive early enough to
  avoid it, under a 3-year training lag (the brief's primary assumption)
  and a 5-year lag (its sensitivity case).
  [Brief](../../docs/briefs/research-brief.md) · [Spec](./spec.md) ·
  [Model](./model.xlsx)

## Known limitation

`model.xlsx` was built by script (openpyxl), not by Excel, so it ships with
every formula uncalculated — it will render blank in a non-recalculating
viewer (e.g. the GitHub.com preview). Open it in Excel or LibreOffice, let
it recalculate, and save before relying on any displayed value. The formula
logic itself was independently verified against a plain-Python simulation
of the same recursion before this file was written (see `prompt-log.md`,
2026-10-09 entry); see `spec.md` for the verification result.

All five `Inputs` values are now real or confirmed: hiring volume and the
decline magnitude come from BLS and the primary paper, transition and
attrition rates are Jennifer's confirmed judgment calls, and
`Base_MidLevel_Stock` is the real weighted mid-level headcount for Graphic
Designers from the IPUMS extract — see `spec.md` §2 for sourcing on each
value and §5 for the verification run (and a correction to an earlier,
wrong claim about which numbers the stock anchor actually affects).
