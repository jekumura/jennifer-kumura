# Economic Research

## What it is

The capability behind the "Missing Rung" research paper: does AI-driven
automation of entry-level tasks weaken the junior-to-mid-level talent
pipeline, and if so, how and on what timeline. Two methods, covering the
paper's two hypotheses:

- **CPS/IPUMS employment-index method** (H1, mechanism) — pull employed
  respondents in target occupations from CPS microdata, build a weighted
  early-career employment index per occupation, and test a directional
  hypothesis between occupations against a pre-specified 95% confidence
  interval rather than the point estimate alone. See `spec.md` §2-4.
- **Pipeline-projection cohort model** (H2, timing) — an Excel cohort
  model: track each hired cohort forward by a training lag N, apply an
  ongoing attrition rate, and compare the resulting stock against a
  baseline (no-shock) path to test whether a hiring recovery arrives in
  time to prevent a mid-level shortage. See `pipeline-model-spec.md`.

## Where it was exercised

- **economic-research** — the paper itself. H1 tests whether Graphic
  Designers' early-career employment declined more than Web & Digital
  Interface Designers' since late 2022, with a CPS-based Home Health Aide
  control. H2 tests whether a mid-level shortage emerges under depressed
  junior hiring, and whether a later hiring recovery arrives early enough
  to avoid it, at a 3-year training lag (primary) and a 5-year lag
  (sensitivity).
  [Brief](../../docs/briefs/2026-10-09-research-brief.md) ·
  [Spec](./spec.md) · [Pipeline model spec](./pipeline-model-spec.md) ·
  [Model](./model.xlsx)

## Known limitation

`model.xlsx` was built by script (openpyxl), not by Excel, so it ships
with every formula uncalculated — it will render blank in a
non-recalculating viewer (e.g. the GitHub.com preview). Open it in Excel
or LibreOffice, let it recalculate, and save before relying on any
displayed value. The formula logic itself was independently verified
against a plain-Python simulation of the same recursion (see
`pipeline-model-spec.md` §5 for the verification result).

All five of the pipeline model's `Inputs` values are now real or
confirmed: hiring volume and the decline magnitude come from BLS and the
primary paper, transition and attrition rates are Jennifer's confirmed
judgment calls, and `Base_MidLevel_Stock` is the real weighted mid-level
headcount for Graphic Designers from the IPUMS extract — see
`pipeline-model-spec.md` §2 for sourcing on each value and §5 for the
verification run.
