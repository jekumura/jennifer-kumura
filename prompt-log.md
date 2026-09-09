# Prompt Log - running record of AI sessions that mattered.

## 2026-09-09 — Spec gap review, Farm Profit Lab validation, model.xlsx built

Asked to evaluate the marginal-analysis spec for guessed-at gaps and
undefined terms (no rewrite), then implement the recommended fixes.
Separately, asked to run five required validation checks against the
model's acceptance criteria (optimal mix, season profit, standalone P≈MC
points) and record findings in spec.md. The first validation pass exposed
a real bug: the spec's own `Marginal_Wage` formula (free farmer hours,
then paid temp hours) didn't reproduce the acceptance criteria at all. A
Farm Profit Lab PDF export (the case's reference implementation) resolved
it — the farmer's hours are the *expensive* tier ($34.72/hr), temp hours
are *cheaper* ($17.36/hr) once 720 hours are exhausted, the exact reverse
of what was specced, and there's no $25,000-per-worker lumpy hiring fee in
the validated model (still an open discrepancy against the brief, which
states one). Fixed §3/§4/§5/§6/§7/§8 accordingly, then rebuilt
`capabilities/marginal-analysis/model.xlsx` from the corrected spec —
which caught a second bug live (a naive "count of profitable beds" formula
overcounted past a marginal-profit dip caused by the wage-tier flip).
Final validated result: Tomatoes 10 / Carrots 20 / Mesclun 30, $42,775
profit, matching the $42,762 target; the greedy P=MC walk and the Solver
optimum now agree exactly.

## 2026-08-24 — Crop economics data filled into the spec

Supplied the case-materials crop economics table (price, fertilizer, labor
hours, diminishing-returns rate per crop) plus the farmer's own salary
terms. Filled in every previously-blank Data Inputs value in spec.md §3,
and added `Farmer_Salary`/`Farmer_Implied_Wage` as a second fixed-cost line
separate from the brief's $20,000.

## 2026-08-24 — capabilities/marginal-analysis/spec.md written

Asked to write the marginal-analysis capability's spec from the
perfect-competition brief. Replaced the spec.md placeholder with model
architecture, named-range data inputs (left blank pending case data),
derived formulas for compounding diminishing returns and the shared-pool
marginal wage, per-crop crossover logic, validation rules, and analysis
requirements tied to the brief's falsification criteria.

## 2026-08-24 — perfect-competition brief added

Asked to add an uploaded engagement brief (market-garden bed allocation
across tomatoes/carrots/mesclun) to `docs/briefs/`. Added
`perfect-competition-brief.md` — scope, fixed/chosen variables,
assumptions, hypothesis, and falsification criteria.

## 2026-08-24 — docs/standards/excel-formatting.md added

Asked to add an uploaded Excel-formatting standard to
`docs/standards/excel-formatting.md` and reference it from `AGENTS.md`,
`CLAUDE.md`, and every capability's `spec.md` so it's explicit per-build,
not just implied. Added the standard and the reference line in all three
places.

## 2026-08-23 — docs/templates/ added

Asked to add a technical-spec template (and its index README) to a new
`docs/templates/` folder, modeled on an external reference repo. Added
`spec-template.md` as an exact copy of the source, and `README.md` adapted
to this repo — trimmed to drop references to sibling templates and course
infrastructure that don't exist here.
