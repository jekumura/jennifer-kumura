---
type: spec
capability: marginal-analysis
engagement: perfect-competition
date: 2026-09-12
version: 1.5
status: committed
---

# Spec — Market Garden Bed Allocation

**Source brief:** [`docs/briefs/perfect-competition-brief.md`](../../docs/briefs/perfect-competition-brief.md)

## 1. Scope & Objective

Determine, before planting, how many of the 64 beds to allocate to each of
three crops (tomatoes, carrots, mesclun) and how many temp workers (0–4) to
hire for the 36-week season — a one-shot decision with no mid-season
correction. The objective is to maximize total season profit (revenue −
labor cost − fertilizer cost − the $20,000 fixed cost). Audience: the
grower making the planting call.

---

## Part A — Model Specification

## 2. Model Architecture

Per `docs/standards/excel-formatting.md`:

- **`Summary`** — chosen bed allocation per crop, temp workers hired, total
  contribution, net profit, hypothesis-verdict checks. First tab.
- **`Inputs`** — every named range in §3, editable/blue.
- **`Calculations`** — per-crop, per-bed marginal labor hours, marginal
  labor cost, marginal cost vs. price, cumulative labor hours, cumulative
  contribution. One block of rows per crop, bed 1 through that crop's cap.
  Below the three per-crop blocks, a separate, clearly-labeled block runs
  the P = MC greedy walk (§5) and reports its resulting allocation and
  re-costed total contribution alongside the Solver optimum.
- **`Solver`** — decision variables (beds per crop, temp workers hired) and
  constraint cells, set up for Excel Solver (this is a constrained,
  non-linear, mixed-integer problem — not a closed-form calculation, so the
  optimum can't just be read off three independent marginal-cost curves).
- **`Notes`** — model description, assumptions from the brief (§ "What I am
  assuming"), data sources, date built.

## 3. Data Inputs

All values below are now sourced from the case materials (crop economics
table) supplied for this engagement.

**Rule:** any input that is itself a computed ratio (a wage per hour
derived from a salary and an hours figure, a rate derived from another
rate, etc.) is entered as a formula referencing its component named
ranges — never as a typed decimal. The case materials' printed number
(e.g. "$34.72/hr") describes that ratio for a human reader, rounded for
display; it is not the value to compute with. Typing the rounded, printed
value in directly loses precision that compounds through every
downstream formula — see the v1.5 correction below, which follows the
same pattern as the `Carrot_Base_Labor_Hrs_Wk_Bed` fix (v1.4).

| Named Range | Source | Value | Unit |
|-------------|--------|-------|------|
| `Total_Beds` | Brief | 64 | beds |
| `Tomato_Bed_Cap` | Case materials | 20 | beds |
| `Carrot_Bed_Cap` | Case materials | 20 | beds |
| `Mesclun_Bed_Cap` | Case materials | 30 | beds |
| `Season_Weeks` | Brief | 36 | weeks |
| `Fixed_Cost` | Brief | 20,000 | $ |
| `Farmer_Season_Salary` | Case materials | 50,000 | $/season |
| `Farmer_Implied_Wage` | Derived — case materials' own wage derivation | `= Farmer_Season_Salary / 2 / Own_Labor_Hours` (34.7222...) | $/hr |
| `Own_Labor_Hours` | Brief | 720 | hrs |
| `Temp_Worker_Max` | Brief | 4 | workers |
| `Temp_Worker_Hours` | Brief | 1,440 | hrs/worker |
| `Temp_Worker_Flat_Cost` | Case materials | 25,000 | $/worker |
| `Temp_Wage_Per_Hour` | Derived — case materials' own wage derivation | `= Temp_Worker_Flat_Cost / Temp_Worker_Hours` (17.3611...) | $/hr |
| `Tomato_Price` | Case materials | 8,800 | $/bed |
| `Carrot_Price` | Case materials | 2,094 | $/bed |
| `Mesclun_Price` | Case materials | 2,700 | $/bed |
| `Tomato_Fertilizer_Per_Bed` | Case materials | 880 | $/bed |
| `Carrot_Fertilizer_Per_Bed` | Case materials | 440 | $/bed |
| `Mesclun_Fertilizer_Per_Bed` | Case materials | 880 | $/bed |
| `Tomato_Base_Labor_Hrs_Wk_Bed` | Case materials | 2.50 | hrs/wk/bed |
| `Carrot_Base_Labor_Hrs_Wk_Bed` | Derived — case defines it as tomato's rate ÷ 3 | `= Tomato_Base_Labor_Hrs_Wk_Bed / 3` (0.8333...) | hrs/wk/bed |
| `Mesclun_Base_Labor_Hrs_Wk_Bed` | Case materials | 1.25 | hrs/wk/bed |
| `Tomato_Diminishing_Rate` | Case materials | 10.00% | %/bed |
| `Carrot_Diminishing_Rate` | Case materials | 2.50% | %/bed |
| `Mesclun_Diminishing_Rate` | Case materials | 1.25% | %/bed |

**Correction (v1.5, per Adam's PR #19 follow-up review):**
`Farmer_Implied_Wage` and `Temp_Wage_Per_Hour` were previously entered as
`34.72` and `17.36` — the case materials' displayed, rounded values —
rather than derived from their own source terms (`$50,000 season salary
÷ 2 ÷ 720 hours` and `$25,000 ÷ 1,440 hours`, respectively). Same class
of error as the `Carrot_Base_Labor_Hrs_Wk_Bed` fix immediately below
(v1.4): a printed number treated as an exact input. This introduces two
new raw constants, `Farmer_Season_Salary` ($50,000) and
`Temp_Worker_Flat_Cost` ($25,000), that exist here purely to derive the
*per-hour wage rate* — `Temp_Worker_Flat_Cost` happens to be the same
$25,000 figure named in the brief's disputed lumpy-hiring-fee (see the
"Open item" note below), but it plays no cost-line role here; it only
feeds this wage-rate ratio. If the lumpy fee is ever confirmed real, it
is added as its own separate term in §4 — not folded into this
derivation.

**Correction (v1.4):** `Carrot_Base_Labor_Hrs_Wk_Bed` was previously entered
as `0.833` — the displayed, rounded value — rather than derived from the
case's own definition (carrot labor = tomato labor ÷ 3 = 0.8333...). Same
class of error as a hardcoded formula constant: a displayed number treated
as an exact input. Now a formula referencing `Tomato_Base_Labor_Hrs_Wk_Bed`
directly, so it can't drift out of sync if that input ever changes.

**Correction (v1.3, per §6's Farm Profit Lab cross-check):** earlier
drafts of this spec treated `Own_Labor_Hours` as a $0-marginal-cost,
already-sunk pool (reasoning that her $50,000 season salary was a fixed
cost paid regardless of the planting decision) and added that $50,000 as
a second fixed-cost line. **Both of those were wrong.** The validated
reference model prices the farmer's *first* 720 hours at
`Farmer_Implied_Wage` ($34.72/hr — the more expensive tier), and only
hours *beyond* 720 drop to the cheaper `Temp_Wage_Per_Hour` ($17.36/hr).
There is no $0 tier, and there is no separate `Farmer_Salary` fixed-cost
line — her compensation is fully captured through the per-hour
`Farmer_Implied_Wage` charge on whichever of her 720 hours actually get
used (see §4). `Fixed_Cost` is the brief's $20,000 alone.

**Kept argument — accounting cost vs. economic cost for the farmer's own
hours:** the case convention (above) charges her 720 field hours into
labor at `Farmer_Implied_Wage` and leaves the rest of her $50,000 salary
out of the model. The alternative this spec originally used — her *whole*
$50,000 salary as a second fixed cost, her 720 hours free at the margin
since they're already paid for — is a legitimate economic-cost reading:
if her salary is a sunk commitment regardless of the planting decision,
only the *incremental* cost of a bed should count, and her own hours
aren't incremental. It just isn't the convention the published check
figures were computed under, so a model built on it can't reach them.

Worth stating precisely what the choice costs, now that the labor formula
itself is fixed (§4): re-solving under the accounting-cost convention with
the corrected labor function gives the **same optimal allocation**,
Tomatoes 10 / Carrots 20 / Mesclun 30 — the reclassification is a pure
lump-sum shift, so it doesn't change which crops are relatively more
profitable at the margin. Only the reported profit moves, to **$17,767**
(down $25,008 from $42,775: +$50,000 fixed cost, −$24,998 no-longer-billed
labor on her 720 hours, netting to roughly that gap). The two conventions
disagree on the season's *bottom line*, not on the *planting decision* —
worth keeping in mind for the Stage 3 memo, where the bottom line is the
point.

**Open item — the brief's stated $25,000-per-worker lumpy hiring fee:**
the brief is explicit and repeated that "$25,000 is owed the moment I
hire one, whether I use 100 of their hours or all 1,440." The validated
Farm Profit Lab's own Season P&L, however, shows no such line item —
`Temp_Workers_Hired` there is an informational/feasibility count only
(how many are needed to cover the hours, capped at 4 → 6,480 max hours),
not a cost driver. §4's active formulas match the validated lab (no
lumpy fee), since that's what §6's acceptance-criteria check confirms.
This is a real, unresolved discrepancy between the brief's narrative and
the reference tool — worth confirming with Adam before treating it as
settled. If the lumpy fee turns out to be real, `Temp_Worker_Flat_Cost`
($25,000/worker, from the brief) needs to come back into §4's cost
formula and the whole optimum re-solved.

**Decision variables** (chosen, not given — no source/value the way data
inputs above have one; these are the Solver tab's changing cells):

| Named Range | Meaning |
|-------------|---------|
| `Beds_Tomato` | Beds planted with tomatoes, 0 ≤ n ≤ `Tomato_Bed_Cap` |
| `Beds_Carrot` | Beds planted with carrots, 0 ≤ n ≤ `Carrot_Bed_Cap` |
| `Beds_Mesclun` | Beds planted with mesclun, 0 ≤ n ≤ `Mesclun_Bed_Cap` |
| `Temp_Workers_Hired` | Temp workers hired, integer 0 ≤ n ≤ `Temp_Worker_Max` |

## 4. Derived Inputs

- **Cumulative labor hours for a crop at q beds (Adam's case-materials
  formula — authoritative):**
  `Cumulative_Labor_Hrs(crop, q) = q × {Crop}_Base_Labor_Hrs_Wk_Bed × Season_Weeks × (1 + {Crop}_Diminishing_Rate)^q`
  — applied to labor hours only, per the brief's assumption; price and
  fertilizer per bed stay flat regardless of bed count. Note this is a
  closed-form total for all `q` beds together, not a sum of independently
  compounding per-bed amounts.
- **Marginal (incremental) labor hours for the q-th bed** — needed for the
  per-bed marginal-cost/crossover analysis in §5, derived as the discrete
  difference of the cumulative formula above (not compounded independently
  per bed):
  `Marginal_Labor_Hrs(crop, q) = Cumulative_Labor_Hrs(crop, q) − Cumulative_Labor_Hrs(crop, q-1)`
  `= {Crop}_Base_Labor_Hrs_Wk_Bed × Season_Weeks × (1 + {Crop}_Diminishing_Rate)^(q-1) × (1 + q × {Crop}_Diminishing_Rate)`
- **Total labor hours needed (shared pool, across all three crops):**
  `Total_Labor_Hours_Needed = Cumulative_Labor_Hrs(tomato, beds(tomato)) + Cumulative_Labor_Hrs(carrot, beds(carrot)) + Cumulative_Labor_Hrs(mesclun, beds(mesclun))`
- **Labor wage tiers (shared pool, two-tier step — corrected in v1.3, see
  §3 and §6):** per the brief's assumption that "an hour is an hour... the
  relevant marginal wage is whichever source is cheaper at the margin" —
  the **first `Own_Labor_Hours` (720) of hours committed, shared across all
  three crops, are the *expensive* tier** (only the farmer's own time is
  available for them, priced at `Farmer_Implied_Wage`, $34.72/hr). **Hours
  beyond that are the *cheap* tier** (temp workers, `Temp_Wage_Per_Hour`,
  $17.36/hr — cheaper than the farmer's own implied rate). There is no
  free/$0 tier. Counterintuitively, this means marginal labor cost *falls*
  once the 720-hour threshold is crossed — a crop that lands its early beds
  before other crops have used up the shared 720 hours pays the expensive
  tier for them; a crop allocated later, after other crops have already
  burned through the 720 hours, gets the cheap tier from its very first
  bed. This is exactly why the standalone per-crop crossover below (which
  can only see its *own* hours, never the other two crops') systematically
  misprices crops relative to the joint/shared-pool reality — see §5.
  For a bed adding `hours_added` on top of `hours_before` hours already
  committed (shared total, before this bed):
  `farmer_portion = MAX(0, MIN(hours_before + hours_added, Own_Labor_Hours) - MIN(hours_before, Own_Labor_Hours))`
  `temp_portion = hours_added - farmer_portion`
  `Marginal_Labor_Cost = farmer_portion × Farmer_Implied_Wage + temp_portion × Temp_Wage_Per_Hour`
  For the standalone per-crop crossover (§5), `hours_before`/`hours_added`
  are that crop's own cumulative hours in isolation. For the greedy walk
  and Solver scenarios, they're the running shared `Total_Labor_Hours_Needed`
  across whichever beds (any crop) have already been committed at that
  point in the walk/solve.
- **Temp workers needed (feasibility count, not a cost — see §3's open
  item on the brief's stated $25,000 lumpy fee):**
  `Temp_Workers_Hired = CEILING(MAX(0, Total_Labor_Hours_Needed - Own_Labor_Hours) / Temp_Worker_Hours, 1)`, capped at `Temp_Worker_Max`. This
  is purely informational/a feasibility bound (`Total_Labor_Hours_Needed`
  must not exceed `Own_Labor_Hours + Temp_Worker_Max × Temp_Worker_Hours`
  = 6,480 hrs) in the validated model — it does **not** add a cost.
- **Total labor cost (aggregate, not per-crop):**
  `Labor_Cost = MIN(Total_Labor_Hours_Needed, Own_Labor_Hours) × Farmer_Implied_Wage + MAX(0, Total_Labor_Hours_Needed - Own_Labor_Hours) × Temp_Wage_Per_Hour`
- **Marginal cost per bed:**
  `Marginal_Cost(crop, n) = Marginal_Labor_Cost(bed n, per above) + {Crop}_Fertilizer_Per_Bed`

## 5. Marginal Analysis Formulas

Because each crop is sold at a flat, quantity-independent price (price
taker), **marginal revenue per bed = `{Crop}_Price`**, constant regardless
of how many beds of that crop are already planted. So for each crop in
isolation:

- **Crossover bed** = smallest `n` such that `Marginal_Cost(crop, n) > {Crop}_Price`
  — the *first* bed where marginal profit turns negative, not a count of
  all profitable beds. This distinction matters here: marginal profit
  isn't monotonic (the tiered wage in §4 makes it dip negative, then jump
  back positive once that crop's own hours cross 720 and the cheaper temp
  rate kicks in), so counting every non-negative bed overcounts — see §6
  for a worked example where this exact bug surfaced during the build.
- **Standalone optimal beds(crop)** = `crossover bed − 1`, capped at that
  crop's bed cap.

This per-crop crossover is the brief's own back-of-envelope first pass
(see its "What I am assuming" note: reasoning about each crop's curve
"mostly independently for this first pass"). It is **not** the final
answer, because:

1. All three crops draw from one shared, capped labor pool (6,480 hrs max),
   so a bed that clears its own crossover may still be uneconomical if it
   displaces a more valuable bed of another crop. Worse, the standalone
   view gets the *direction* of the error wrong in this specific model: per
   §4, the shared pool's first 720 hours are the *expensive* tier and hours
   beyond are *cheap*, so a crop evaluated alone (forced to pay the
   expensive tier for its own first 720 hours) looks systematically worse
   than it would if allocated after other crops have already absorbed that
   expensive tier. This is why mesclun's standalone crossover (~bed 6-7,
   §6) is nowhere near its joint-optimal 30-bed allocation.
2. *If* the brief's stated $25,000-per-worker lumpy hiring fee turns out to
   be part of the real cost model (open item, §3), it would be lumpy across
   the whole allocation, not per-crop — the 3rd or 4th worker's fee would
   need to be justified by the marginal value of the hours it unlocks
   across all three crops combined, not any single crop's curve. The
   validated reference model (§6) doesn't charge this fee, so it isn't
   live in §4's active formulas right now — but if it comes back, this
   reasoning (and the greedy-walk-vs-Solver divergence risk below) applies
   again.

### The brief's stated mechanism: a P = MC greedy walk

The brief's Hypothesis section names a specific allocation mechanism,
distinct from the standalone per-crop crossover above: **allocate the next
bed to whichever crop currently has the highest marginal profit** (price
minus that crop's marginal cost at its current bed count), and repeat
**until either all 64 beds are filled or no crop's next bed has positive
marginal profit — whichever happens first.** (The brief's own "push a crop
past the point where its next bed costs more than it earns... leaving
money on the table" framing rules out continuing to allocate once every
remaining option is unprofitable, even if beds are still available.) This
interleaves all three crops bed-by-bed instead of
solving each crop's curve in isolation — it is the mechanism behind the
brief's current hypothesis (mesclun and carrots each saturating their own
bed caps, tomatoes taking the remainder).

The model must compute this greedy walk explicitly (as its own labeled
scenario, separate from both the standalone crossover and the Solver
optimum) and report it alongside the Solver optimum so the two can be
compared directly. **Under the validated cost model (§6 — no lumpy
hiring fee), the greedy walk is confirmed to reach exactly the same
allocation as the true joint optimum** (Tomatoes 10 / Carrots 20 / Mesclun
30, $42,775): each bed's marginal cost depends only on the shared running
total of hours committed so far, which rises smoothly (a two-tier step,
not a discontinuous jump), so always taking the best available next bed is
optimal here. This equivalence is *not* guaranteed in general, though —
if the brief's stated $25,000-per-worker lumpy fee turns out to be real
(§3's open item), the greedy walk would stop weighing it correctly (it
only ever "sees" the flat per-hour wage, never a fixed hiring fee it's
about to trigger) and could diverge from the true optimum again. Keep
computing both scenarios even though they currently agree, so a future
change to the cost model surfaces as a visible disagreement rather than a
silent one.

The actual optimum requires jointly solving beds-per-crop and
temp-workers-hired via the `Solver` tab (integer/non-linear constraints:
per-crop bed caps, total bed cap, total labor hours, integer 0–4 temp
workers), maximizing total contribution. The per-crop crossover formulas
and the greedy walk above both exist to sanity-check Solver's output
against intuition — and against the brief's own stated hypothesis and
mechanism — not to replace it.

## 6. Validation Rules

Apply every rule below to **all three computed scenarios** — the
standalone per-crop crossover, the greedy P = MC walk, and the Solver
optimum (§5) — not just to Solver's decision cells.

- `SUM(beds per crop) <= Total_Beds`
- `beds(crop) <= {Crop}_Bed_Cap` for each crop
- `Temp_Workers_Hired` is a non-negative integer `<= Temp_Worker_Max`
- `Total_Labor_Hours_Needed <= Own_Labor_Hours + Temp_Workers_Hired × Temp_Worker_Hours`
- **Per-crop contribution**, for the check cell below — defined as revenue
  minus fertilizer cost only (`beds(crop) × {Crop}_Price − beds(crop) ×
  {Crop}_Fertilizer_Per_Bed`), since labor cost is **not** attributable to
  a single crop under the shared-pool wage-tier logic in §4. Labor cost is
  subtracted once, in aggregate, never split across crops.
- Check cell: `Summary` profit reconciles to
  `SUM(per-crop contribution) − Labor_Cost − Fixed_Cost`
  computed independently on `Calculations`. (No `Temp_Hiring_Cost` or
  `Farmer_Salary` term — see §3/§4's v1.3 correction.)

### Acceptance Criteria

AI output is a draft, not a deliverable — these are published on purpose,
before the build, so they're a test suite the model must pass, not an
answer key to reverse-engineer into. Run all five whenever the model
changes (a formula edit, a data update, a rebuild), and don't call a build
done until every one passes.

| # | Check | What it catches |
|---|-------|------------------|
| 1 | **q=1 by hand.** `Cumulative_Labor_Hrs(tomato,1)` should equal `1 × 2.50 × 36 × 1.10 = 99.0` hrs. | A dropped exponent or off-by-one in §4's compounding formula — the single most common structural defect, and invisible downstream. |
| 2 | **Cross-check against the Farm Profit Lab** (`adamwstauffer.github.io/ai-lms/farmlab.html`) — take at least one intermediate marginal cost and compare. | Anything wrong in the middle of the model. The lab is a separate implementation of this same model, so agreement is real evidence. |
| 3 | **Two Solver starting points** — 0/0/0 and 20/0/0 (`Beds_Tomato`/`Beds_Carrot`/`Beds_Mesclun`). | A local optimum masquerading as the answer. If they disagree, that's a finding, not a nuisance — and 20/0/0 is infeasible outright (§4/§8), which is itself worth knowing before relying on Solver. |
| 4 | **The check figures** (table below). | Everything else, in aggregate. |
| 5 | **Formulas, not pasted values** — spot-check both `Inputs` (any named range that is itself a derived ratio, not a raw case constant) and `Calculations` cells, confirming each references other named ranges in §3 rather than a hardcoded number. | A number that's right today and wrong the moment an input changes — including a derived value sitting on `Inputs` itself, not only a formula on `Calculations`. |

**Check figures (acceptance targets for check 4), each with a tolerance:**

| Check | Value | Tolerance |
|-------|-------|-----------|
| Optimal mix | Tomatoes 10 · Carrots 20 · Mesclun 30 (60 beds) | Exact — integer beds, no rounding room |
| Season profit | $42,761.66 | ± $0.01 (cent-level display rounding only — once §3's v1.5 wage-formula fix is in place, the model should reproduce this exactly; no other slack is justified) |
| Temp workers needed | ~3.16 (uncapped/unrounded — `(Total_Labor_Hours_Needed − Own_Labor_Hours) / Temp_Worker_Hours`; `Temp_Workers_Hired` itself rounds this up to the integer 4, since you can't hire a fraction of a worker) | ± 0.05 |
| Standalone P ≈ MC points | Tomatoes ~10 · Carrots ~10 · Mesclun ~6 beds | ± 1 bed each |
| q = 1 hand check | `Cumulative_Labor_Hrs(tomato,1)` = exactly 99.00 hrs | Exact |

A model that reproduces these numbers with broken formulas fails
inspection just as surely as one that misses them — check 5 exists
specifically to catch that. When a check fails, fix the spec and
regenerate the workbook; don't patch the workbook by hand. A workbook
that no longer matches its spec is a model nobody can rebuild.

### Audit Findings (last updated 2026-09-09)

Ran against the committed `capabilities/marginal-analysis/model.xlsx`,
recalculated fresh via LibreOffice (1,080 formulas, 0 errors).

| Check | Result |
|-------|--------|
| 1 — q=1 by hand | **Pass.** `Calculations!C5` (tomato bed 1 marginal hours) = 99.0, matching `1 × 2.50 × 36 × 1.10` exactly. |
| 4 — The check figures | **Pass.** `Summary` shows Tomatoes 10 / Carrots 20 / Mesclun 30 (60 beds), PROFIT $42,768.33 — within the ±$50 tolerance of the $42,762 target (the earlier $13 residual, from `Carrot_Base_Labor_Hrs_Wk_Bed` being a rounded literal, is most of the way closed now that it's a formula — see the defect record below). Temp workers needed, unrounded: `Calculations!D48` = 3.16, matching the ~3.16 target. Standalone crossovers on `Calculations`: Tomato bed 10, Carrot bed 10, Mesclun bed 6 (`F26`/`M26`/`T36`) — exactly the ~10/~10/~6 target. |
| 5 — Formulas, not pasted values | **Pass.** `Calculations` cells reference `Tomato_Base_Labor_Hrs_Wk_Bed`, `Season_Weeks`, `Tomato_Diminishing_Rate`, etc. by name throughout. `Carrot_Base_Labor_Hrs_Wk_Bed` itself is now a formula (`=Tomato_Base_Labor_Hrs_Wk_Bed/3`) rather than the rounded literal `0.833` it used to be — see below. |

Two more, for completeness:

| Check | Result |
|-------|--------|
| 2 — Farm Profit Lab cross-check | **Pass.** The lab's displayed "+1 bed" marginal profits (Tomatoes +$4,483, Carrots +$586, Mesclun +$238, from a `farmlab.html` export) match this model's own bed-1 marginal contributions to the cent. |
| 3 — Two Solver starting points, 0/0/0 and 20/0/0 | **Pass, with a caveat.** 0/0/0 converges (via hill-climbing) to the true optimum, Tomatoes 10/Carrots 20/Mesclun 30. 20/0/0 is infeasible outright — 20 tomato beds alone need ~12,110 labor hours against the 6,480-hour max — which is a real Solver gotcha worth knowing (don't start there), not a defect in the cost model. |

All five checks pass. `Calculations!D153` (greedy walk vs. Solver PROFIT
delta) reads 0 — the two methods agree exactly.

**Defect record — instructor review, [PR #11](https://github.com/jekumura/jennifer-kumura/pull/11):**
a prior version of this spec (checked against the same five checks, before
the Farm Profit Lab cross-check existed) had two costing defects an
instructor review caught that these checks, as they stood then, did not:

1. **Temp labor charged twice** — the old model summed a per-hour temp
   wage on hours beyond 720 *and* a separate $25,000-per-worker flat fee
   for the same hours, contradicting its own Notes tab, which called the
   fee "lumpy... not a per-hour charge." Caught by: nothing at the time —
   §6 had no published check figures yet, so a $51,000 gap between the
   model's answer and the case's had nothing to trip against. Fixed by
   removing the flat fee from the active cost formula entirely (§4) once
   the Farm Profit Lab confirmed the validated model doesn't charge it.
2. **Diminishing returns applied to only the newest bed, not the whole
   crop** — the old formula compounded per-bed (`base × 36 × (1+rate)^(n-1)`,
   summed down the column) instead of the case's closed form
   (`q × base × 36 × (1+rate)^q`, applied to all `q` beds at once). At 10
   tomato beds this was a 900-hour understatement (1,434 vs. 2,334). Caught
   by: check 1 (q=1 by hand) would have caught it immediately if it had
   existed then — both formulas agree trivially at `q=1`, so the bug only
   shows up once you check a real quantity, which is exactly why check 1
   is specified as a hand-computed value, not just "run it and see." Fixed
   by swapping in the case's closed-form formula (§4).
3. **`Carrot_Base_Labor_Hrs_Wk_Bed` entered as a rounded literal** — `0.833`
   typed in directly, rather than derived from the case's own definition
   (carrot labor = tomato labor ÷ 3 = 0.8333...). Caught by: check 5
   (formulas, not pasted values) — a displayed number treated as an exact
   input is precisely what that check exists to catch, and it should have
   been checked against §3's own inputs, not just the `Calculations`
   formulas. Fixed by entering it as `=Tomato_Base_Labor_Hrs_Wk_Bed/3` (§3).

All three are now closed: `model.xlsx` rebuilt from the corrected spec,
recalculated clean, and all five checks re-run and passing (above).

**Defect record — instructor review, PR #19 follow-up (2026-09-12):**
a second review, run after the three fixes above, found one more costing
defect and two process gaps that let it through unnoticed.

4. **`Farmer_Implied_Wage` and `Temp_Wage_Per_Hour` entered as rounded
   literals** — `34.72` and `17.36` typed in directly, rather than
   derived from the case's own wage terms (`$50,000 ÷ 2 ÷ 720 hours` and
   `$25,000 ÷ 1,440 hours`). This is the same defect class as item 3
   above, recurring in a different cell — a printed value stood in for a
   computation. It moved season profit by $6.67 ($42,768.33 vs. the true
   $42,761.66), small enough to hide inside a tolerance band rather than
   trip an exact check. Caught by: nothing at the time — check 5 only
   scanned `Calculations`, and these two literals live on `Inputs`, so
   there was no check positioned to see them at all. Fixed by entering
   both as formulas referencing new raw constants `Farmer_Season_Salary`
   and `Temp_Worker_Flat_Cost` (§3, v1.5).
5. **Check 4's ±$50 tolerance was added after the check failed, not before
   it was run** — it existed to let a $13 (later $6.67) known gap pass
   rather than to accommodate a pre-stated source of imprecision. A
   tolerance justified only by "the check currently fails without it" is
   not evidence the model is correct; it's evidence the check was
   loosened to match the answer. Caught by: nothing internal — an
   external review had to point out the tolerance had no standing
   justification. Fixed by removing it: once defect 4 is closed, check 4
   requires an exact match to the cent (§6 check-figures table).
6. **Check 5 was scoped only to `Calculations`, structurally unable to see
   `Inputs`** — the two literals in defect 4 sat on the one sheet the
   check never looked at. This is the same blind spot as defect 3's
   "Caught by" note already flagged, but it was never actually fixed in
   the check's own scope — only the one input it happened to catch was
   fixed. Caught by: nothing internal, for the same reason as defect 4.
   Fixed by widening check 5 to explicitly cover `Inputs` (§6).

All six defects are now closed on paper (this spec). `model.xlsx` has not
yet been regenerated from this version — do that next, then re-run all
five checks and update the check-figures/Audit Findings results above
(they currently reflect the pre-v1.5 build and are stale as of this
edit).

---

## Part B — Analysis Specification

## 7. Analysis Requirements

The current hypothesis (brief frontmatter: "mesclun-max and carrot max mix;
exhaust the highest marginal profit crops up until their limits") is that
**mesclun and carrots each saturate their own bed caps** — mesclun ~45% =
30 beds (its cap), carrots ~30% = 20 beds (its cap) — with tomatoes taking
the remaining ~25% (~10 beds), on the combination of high diminishing
returns, high labor rate, and high fertilizer cost. The stated mechanism is
the P = MC greedy walk described in §5. Under the validated cost model
(§6), the greedy walk and the Solver optimum reach the same allocation, so
the analysis can check the brief's five falsification conditions against
either (they should agree — see §5 for when that could stop being true):

1. Does carrots end up with **more** beds than mesclun? If so, mesclun's
   price + low-diminishing-returns edge doesn't survive its own curve.
2. Zero out the fertilizer-cost gap across crops — set all three crops'
   `{Crop}_Fertilizer_Per_Bed` to the **average of the current three
   values** — and re-solve. Do tomatoes still land last? If yes,
   fertilizer cost wasn't doing real explanatory work.
3. Zero out the labor-rate gap across crops — set all three crops'
   `{Crop}_Base_Labor_Hrs_Wk_Bed` to the **average of the current three
   values** — and re-solve. Do tomatoes still land last? If yes, labor
   rate wasn't load-bearing either, and the ranking is driven by the
   diminishing-returns curve shape alone.
4. Does mesclun's marginal per-bed return survive all the way to its
   **30-bed cap**? If its marginal return crosses *below* carrots' before
   bed 30, the mesclun-max claim is falsified — the brief now pins this
   threshold to mesclun's own cap rather than leaving it open.
5. At the optimum, does moving one bed from mesclun to carrots raise total
   contribution? If so, the binding constraint (labor hours or total beds)
   rewards a more even split, not mesclun-heavy concentration.

## 8. Constraint Diagnosis

Identify which constraint actually binds at the optimum — a crop's own bed
cap, or the 6,480-hour labor ceiling (`Temp_Worker_Max` × `Temp_Worker_Hours`
+ `Own_Labor_Hours`) — and report the sensitivity of relaxing each binding
constraint by one unit (one more bed of cap, one more labor hour). At the
validated optimum (Tomatoes 10/Carrots 20/Mesclun 30, §6), it's the two
bed caps that bind — Carrots and Mesclun both sit at their maximums while
Tomatoes stops well short of its own (10 of 20) and total labor hours
(~5,277) stay well under the 6,480 ceiling. Compute sensitivity
**manually**: relax the constraint by one unit,
re-solve, and take the difference in `Total_Contribution` — not via
Excel Solver's built-in Sensitivity Report. That report's shadow prices
are only well-defined for continuous linear problems, and this model has
integer constraints on both beds and `Temp_Workers_Hired`; Solver will
generally refuse to produce a meaningful sensitivity report for it.

## 9. Strategic Recommendations

- Final beds-per-crop allocation and temp-worker hiring decision.
- Explicit verdict on the hypothesis: confirmed, or which of the five
  falsification conditions in §7 tripped and what that implies about the
  "mesclun-max and carrot-max mix, exhausting the highest-marginal-profit
  crops up to their limits" story.
- Explicit comparison of the greedy P=MC walk (§5) against the Solver
  optimum — under the validated cost model they agree exactly (§6); if a
  future run disagrees, that's a signal the cost model changed (e.g. the
  brief's lumpy hiring fee coming back into scope, per §3/§5) and needs
  investigating, not averaging over.
- Sensitivity notes from §8 (what changes the recommendation if a
  constraint were relaxed).

## 10. Output Format

1. Allocation table (beds per crop, temp workers hired) — Solver optimum
   and greedy P=MC walk (§5), side by side.
2. Total contribution and net profit for both.
3. Hypothesis verdict against each of the five §7 checks.
4. Binding constraint and its sensitivity (§8).

The `Summary` tab holds the numbers above; the recommendation itself —
addressed to the grower, with the verdict and the binding-constraint story
in prose — belongs in a `docs/decisions/` entry, per this repo's own
convention (`AGENTS.md`: written after work finishes, addressed to a
specific audience). The Summary tab is the evidence; it doesn't replace
the decision doc.

---

## References

- [`docs/briefs/perfect-competition-brief.md`](../../docs/briefs/perfect-competition-brief.md) — engagement brief (problem framing, fixed/chosen variables, assumptions, hypothesis, falsification criteria)
- [`docs/standards/excel-formatting.md`](../../docs/standards/excel-formatting.md) — workbook build conventions

