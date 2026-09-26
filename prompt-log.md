# Prompt Log - running record of AI sessions that mattered.

## 2026-09-25 — research-brief.md thesis: voice critique, then swapped in

Tool: Claude Code. Wrote the initial research-brief.md scaffold myself
(economic-research engagement, Stage 1 · Ask) and asked Claude only to
save it to file — full authorship was mine from the start, unlike the
perfect-competition analysis/memo earlier. Later asked Claude to
"rewrite it in my own words," which it declined (rewriting content for
me would be the same authorship problem as before, just relabeled), and
instead offered to point at specific sentences that read AI-scaffolded.
It flagged three: the thesis's compressed "too late" cadence, a
"professional judgment" phrase repeated near-verbatim in two places, and
uniform bullet rhythm in two list sections. I rewrote the thesis
paragraph myself. Claude's structural check flagged one real issue: my
rewrite hedges twice ("may... may...") where the brief's own "Contested
question" section states the same position unhedged — a genuine
inconsistency to resolve, not a style nitpick. Swapped my rewritten
thesis into the brief, replacing the scaffold version.

## 2026-09-25 — Instructor feedback: figures embedded, log correction

Tool: Claude Code. Pasted instructor feedback on the Stage 2/3 submission
(overall 9.1, five listed fixes) and asked Claude to work through it.
Embedded both figures as Markdown images in
`analysis/perfect-competition-analysis.md` where the text already
referenced them, and removed the closing figure-path list. Corrected the
2026-09-09 entry below, which had described a $42,775 result as
"matching" a $42,762 target when it was actually a $13 gap closed by a
later fix.

Claude also independently verified the feedback's central claim — that
the AVC sentence in `analysis/perfect-competition-analysis.md` §4
overclaims, since mesclun's own standalone AVC exceeds its $2,700 price
at beds 13–14 ($2,716.35 and $2,702.51) — and drafted an AVC column for
`capabilities/marginal-analysis/model.xlsx` plus a `spec.md` v1.6
addendum documenting it. Scoped out of this commit at the user's
instruction (analysis and prompt-log only, this round); that work stays
uncommitted pending a separate decision.

This round did not touch the AVC sentence itself or the reflection the
feedback asks for — both are analysis/judgment content reserved for the
user under this stage's rules; Claude gave cell references and figures
for the sentence and left it and the reflection for the user to write.

## 2026-09-23 — MC-dip paragraph rewritten, verified, and swapped into the analysis

Tool: Claude Code. After the structural critique below, asked Claude only
for the specific cell locations needed to check the draft's claims myself
(bed 5/6 cumulative hours, the wage rates, whether bed 6 splits tiers) —
not the values. Wrote my own revised paragraph citing the two cumulative-
hours figures (724.73, 956.64) and both wage rates ($34.72/$17.36).
Claude verified the revision against the model: confirmed the cumulative-
hours figures against `Calculations!D9`/`D10`, and independently ran the
farmer/temp split formula from `spec.md` §4 to confirm bed 6's marginal
hours land entirely in the temp tier (not an approximation — the formula
returns exactly 0 farmer hours for bed 6 once cumulative already exceeds
720). No factual errors found. Swapped this paragraph into
`analysis/perfect-competition-analysis.md` §3, replacing the AI-authored
explanation that had been there since the file was first written.

## 2026-09-23 — Structural critique of MC-dip draft paragraph

Tool: Claude Code. Asked Claude to critique — not rewrite — a self-written
draft paragraph explaining the tomato MC dip around bed 6, following this
stage's rule that AI may edit structure on a committed draft but not
author content. Claude returned structural notes only: vague quantifiers
("around bed 6," "about 725") standing in for exact bed numbers and cell
references, a logical gap in which bed's cumulative hours actually cross
the 720-hour threshold, an uncited assumption about labor-priority order,
"average" used where "marginal" was meant, the two wage rates missing
entirely, and an optional reordering suggestion. No rewritten prose or
substitute numbers were supplied. Revision is mine to write and commit
separately.

## 2026-09-23 — Decision memo split into its own PR

Tool: Claude Code (GitHub MCP). Asked to move the already-committed
decision memo out of the branch for the now-merged PR #23 into its own
pull request. Rebased the two memo-only commits onto current `main`
(templates/analysis/figures had already merged there), pushed under a new
branch name after a force-push attempt was blocked by the harness, and
opened PR #24 containing only `docs/decisions/perfect-competition-memo.md`.

## 2026-09-23 — perfect-competition-memo.md written and revised

Tool: Claude Code. Asked Claude to write the Stage 3 decision memo
directly against `memo-template.md`'s structure. This was full AI
authorship of the recommendation, the three reasons, the judgment call,
and the sensitivity line — not an edit of a prior draft of mine. Numbers
(shadow prices, MC crossover, labor-hour slack) were pulled from the
already-built model and the Stage 2 analysis below. Committed, then asked
Claude to tighten the prose into flowing paragraphs matching the
template; revised version committed separately.

## 2026-09-19 — perfect-competition-analysis.md written, two figures generated

Tool: Claude Code. Asked Claude to write the four-question Stage 2
analysis directly against `capabilities/marginal-analysis/model.xlsx` —
again full AI authorship, not an edit of a prior draft of mine. Claude
read the workbook's Calculations/Summary/Solver tabs directly, computed
the two constraint shadow prices manually (relaxing each bed cap by one
and re-solving, since the model doesn't store them), and generated two
MC-vs-price figures from the workbook's own data into `analysis/figures/`.
Committed with the figures.

## 2026-09-19 — brief-template.md and memo-template.md added

Tool: Claude Code. Asked to add two new deliverable templates to
`docs/templates/`, matching a structure supplied inline (frontmatter
fields plus section headers) rather than an external reference repo.
Added both, matching the existing `spec-template.md` convention.
`memo-template.md` became the structural basis for the actual decision
memo above.

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
Final result at this point: Tomatoes 10 / Carrots 20 / Mesclun 30,
$42,775 profit — a $13 gap against the $42,762 target, not a match; the
gap was inside the tolerance band that existed at the time, and was
fully closed later by the v1.5 wage-formula fix (see the 2026-09-23
entries above), which brought profit to the exact $42,761.66 target with
no tolerance needed. The greedy P=MC walk and the Solver optimum already
agreed exactly at this point, independent of that later fix.

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
