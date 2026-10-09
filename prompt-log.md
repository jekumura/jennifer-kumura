# Prompt Log - running record of AI sessions that mattered.

## Reflection

AI helped me build and troubleshoot the model, but I found that its
outputs still needed to be checked against the underlying assumptions
and source material. On September 9, the AI-assisted wage-tier formula
reversed the farmer and temporary-worker wage rates. I caught this by
comparing the model against the Farm Profit Lab PDF, which specified the
correct wage treatment, and corrected the formula. Later that same
session, a formula counting profitable beds continued counting beds past
the wage-tier dip; I investigated the formula because the resulting
count did not align with the model's expected behavior and rebuilt the
calculation. On September 23, I had a different experience: I wrote the
explanation of the marginal-cost dip myself, and the AI verified my
reasoning against Calculations!D9 and D10 and the specification's
formula. That verification confirmed that the reasoning was supported by
the model rather than identifying an AI error. Together, these
experiences showed me that AI was most useful as a tool for analysis and
verification, but I needed to independently check its outputs against
the model, source material, and underlying formulas.

## 2026-10-09 — capabilities/pipeline-projection built; a pasted spreadsheet design declined, then sanity-checked

Tool: Claude Code. Working through loose end 4 of the 2026-10-06 review
("when the cohort model is built, answer in H2 which year the shortage
appears at three years and at five"), I pasted a fully-structured
spreadsheet design from another AI tool — assumptions layout, a
cohort-tracking formula, a recommendation to keep N=5 as primary. Claude
flagged the formatting (raw LaTeX sitting next to a garbled rendering of
itself) as a tell this wasn't typed directly, the same pattern as the
"Claim 1-5" chain earlier this session, and asked before engaging with it
as a plan. I confirmed it was pasted and asked for a sanity check only, not
adoption.

The sanity check caught two real issues, not just style: the core
`MidSupply` formula named attrition as an input but never actually used it
— no year-over-year decay was wired into the math, so the stock as written
would only ever grow. And the suggestion to keep N=5 as the "original
committed assumption" with N=3 as a sensitivity test was stale — it didn't
know I'd already formally decided the reverse several rounds earlier, so
following it would have quietly reverted a decision already recorded in
the brief.

We then derived the corrected recursive formula together from scratch:
`Graduates[t,s,N] = Hire[t-N,s] x Transition_Rate`, with `MidSupply`
properly recursive and attrition applied after the new cohort joins the
stock (my choice, after Claude asked whether I wanted it before or after).
Claude built the actual workbook — `capabilities/pipeline-projection/`
(`spec.md`, `model.xlsx`, `README.md`) — from those exact formulas,
following `docs/standards/excel-formatting.md`, and independently verified
the formula logic against a plain-Python simulation before writing it
into Excel, since this environment can't recalculate the workbook itself
(same known limitation as `capabilities/marginal-analysis/model.xlsx`).
Every Inputs-tab value is an explicitly-labeled placeholder; the real
assumption values, and the actual H2 answer at N=3 versus N=5, are mine to
supply and read once I have real figures to put in.

## 2026-10-06 — research-brief.md: four items from Adam's 2026-10-01 review resolved

Tool: Claude Code. Worked through the four ordered items from Adam's
2026-10-01 pre-deadline review in sequence.

**Mixed-source TBD.** Claude researched the actual age-band definitions
behind the Stanford/ADP and CPS series (both use 22-25, confirmed via
the primary paper) and worked out a rough CPS cell-size estimate for
the two design occupations (roughly 13-20 respondents per quarter,
back-of-envelope) against the two large occupations' far larger cells.
I weighed switching all four occupations to CPS for source consistency
against keeping the mixed design for the ADP figure's external,
citable backing, and chose to keep ADP. Wrote the "Why this design"
rewrite myself in two drafted passes; Claude caught a real
inconsistency between my first draft's hedged "contextual evidence"
framing and the section's pre-existing unhedged "the control rules out
the boring explanation" line, which I resolved by softening the
control's claim. Decided to cut the "software developers anchor the
pattern" bullet, then restored it with new framing before the edit
landed.

**Decision-owner.** Claude pointed out that the thesis's own wording
("firms need mechanisms for collectively investing...") already
implied firms as the owner, even though the frontmatter listed two
co-equal actors and the recommendation directions spread across four.
I decided firms are the decision-owner and professional associations
are the coordinating vehicle, not a separate actor, and wrote the
frontmatter and Recommendation-directions rewrite myself.

**Progression-time reconciliation.** Claude searched for a citable
source for junior-to-mid-level progression time and found none that
met the brief's own citable-source guardrail (only career-blog
aggregators), but surfaced that those sources distinguish a shorter
junior-to-mid timeline from a longer junior-to-senior one. I decided on
three years as my professional-judgment primary assumption, with five
years as a sensitivity case, reversing which number the pipeline model
treats as primary. Claude flagged H2's "shortage by 2030" claim as
provisional, since no pipeline model has actually been built yet and
the claim predates this change.

**Elasticity bullet.** Claude identified that the bullet's closing
clause ("when quantity can't adjust, price does") was a leftover from
the self-correction/price mechanism I'd already cut in the 2026-10-01
wage-to-hiring reframe below, and asked what actually adjusts in the
current story. I wrote the replacement myself, describing hiring
quantity adjusting with a multi-year lag instead of price substituting.

**Page-budget triage.** Claude mapped which of the six listed economic
concepts were actually tested by one of my three planned analyses
versus along for the ride, and which of the two figures Adam's own
"if Figure 1 needs the room" question pointed at. I decided to cut
tragedy of the commons, who gained and who paid, and the macro link
(none tested by an analysis), keeping externalities, elasticity, and
the specificity rule — each now stating the job it does in the
argument. Designated Figure 2 (the pipeline projection) as the
appendix candidate, keeping Figure 1 as the required main-body
evidence.

All five decisions above are mine; Claude's role was limited to
research (age-band and source-type facts, cell-size and citation
checks), pointing out structural inconsistencies, and mechanically
placing wording I supplied once I'd decided. Delivered as PR #38.

## 2026-10-01 — research-brief.md: Anthropic Job Explorer citation confirmed

Tool: Claude Code. I checked the Anthropic Economic Index Job Explorer
directly and confirmed it classifies Graphic Designers and Web &
Digital Interface Designers separately, each with its own
automation/augmentation measure. Claude recorded that confirmed finding
in the assumptions table and "Data to gather," replacing the earlier
`[CHECK: occupation- or task-level automation vs. augmentation data]`
placeholder, and scoped a narrower `[TBD]` for the one thing still
unconfirmed: the actual percentages behind the automatability ranking,
not just that the data exists. Delivered as PR #35.

## 2026-10-01 — research-brief.md: Stanford data-availability finding and CPS/IPUMS fallback plan recorded

Tool: Claude Code. After evaluating and rejecting Lightcast and the
LinkedIn Economic Graph on licensing/access grounds, I checked the
Stanford Canaries Dashboard directly and confirmed its public interface
exposes early-career series for Software Developers and Home Health
Aides but not for Graphic Designers or Web & Digital Interface
Designers. Decided to measure the two design occupations via CPS/IPUMS
microdata instead, preserving the four-occupation design rather than
narrowing the comparison. Created an IPUMS account and asked Claude to
check extract cell sizes and consider quarterly pooling.

Claude verified the CPS occupation-code boundary before I built on it:
Web & Digital Interface Designers was merged with Web Developers under
one code through 2019 and only split cleanly into its own code (1032)
from 2020 onward, which matters because the brief's "late 2022 to
present" window needed to fall entirely inside the clean-separation
period to avoid a real measurement-precision error — it does. Claude
recorded the data-availability finding and the fallback plan in the
brief and left the mixed-source implication as a `[TBD]` bracket,
matching the brief's existing convention, since that's a judgment call
for me to make, not a fact to record (resolved in the 2026-10-06 entry
above). Delivered as PR #34.

## 2026-10-01 — research-brief.md: wage mechanism reframed to hiring, seven passages rewritten

Tool: Claude Code. Decided to cut the self-correction/price-signal
argument from the brief entirely rather than just dropping the wage data
step; Claude mapped every place in the document the wage mechanism
touched (thesis, the contested-question section, the analysis plan,
both figures, H2, one falsification bullet, the obvious-objection line —
seven spots in total, not one) before any editing started, since the
scope was larger than initially described. Reframed self-correction
around junior-hiring recovery instead of wages, consistent with the
Stanford finding ("adjustment has come through employment, not wages")
already cited in the brief.

Wrote all seven passages myself, one at a time; Claude's role was
structural critique only — no content supplied. Two real logical gaps
were caught and fixed in the process: an early H2 draft referenced
"junior hiring beginning to recover" inside the Depressed scenario,
which is self-contradictory since that scenario is defined as hiring
staying flat; and the "case against self-correction" bullet initially
reused "adjustment has come through employment, not wages" as evidence
against recovery, when employment is now the recovery channel itself,
not just the channel of the original decline — both were caught before
being committed, not after.

Also revisited N (junior-to-mid-level progression, the pipeline model's
key parameter) and the shortage year: considered moving to N=5/2035,
decided against changing the year (2030 matches the Depressed scenario's
own defined window; 2035 would not), kept N at 5. Swapped all seven
rewritten passages and the analysis-plan restructure into
research-brief.md once verified.

## 2026-09-28 — AVC sentence rescoped, then swapped in

Tool: Claude Code. Adam's 2026-09-24 review (PR #26) flagged the AVC
sentence in analysis.md §4 for overclaiming ("every bed count checked")
when mesclun's AVC actually exceeds its price at beds 13–14. Claude gave
cell references from the AVC column added earlier, then — when asked to
help write the sentence directly — declined and instead asked a
question ("does the bump ever get planted, and what does it coming back
under price by bed 30 tell you?") to prompt me to construct the "why it
doesn't matter" reasoning myself rather than supplying it. I drafted the
scoped sentence in two passes; Claude's second check went beyond my own
claim, independently confirming beds 13–14 are the *only* excursion
above price across all 30 mesclun beds, not just checking the numbers I
cited. Swapped my sentence into analysis.md, replacing the original
overclaiming version.

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
