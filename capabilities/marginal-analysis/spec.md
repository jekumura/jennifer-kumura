---
type: spec
capability: marginal-analysis
engagement: perfect-competition
date: 2026-09-09
version: 1.2
status: draft
---

# Spec — Market Garden Bed Allocation

**Source brief:** [`docs/briefs/perfect-competition-brief.md`](../../docs/briefs/perfect-competition-brief.md)

## 1. Scope & Objective

Determine, before planting, how many of the 64 beds to allocate to each of
three crops (tomatoes, carrots, mesclun) and how many temp workers (0–4) to
hire for the 36-week season — a one-shot decision with no mid-season
correction. The objective is to maximize total season contribution
(revenue − labor cost − fertilizer cost) net of temp-worker hiring costs and
the two fixed-cost lines ($20,000 general + $50,000 farmer salary — see §3).
Audience: the grower making the planting call.

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

| Named Range | Source | Value | Unit |
|-------------|--------|-------|------|
| `Total_Beds` | Brief | 64 | beds |
| `Tomato_Bed_Cap` | Case materials | 20 | beds |
| `Carrot_Bed_Cap` | Case materials | 20 | beds |
| `Mesclun_Bed_Cap` | Case materials | 30 | beds |
| `Season_Weeks` | Brief | 36 | weeks |
| `Fixed_Cost` | Brief | 20,000 | $ |
| `Farmer_Salary` | Case materials | 50,000 | $/season |
| `Farmer_Implied_Wage` | Case materials | 34.72 | $/hr |
| `Own_Labor_Hours` | Brief | 720 | hrs |
| `Temp_Worker_Max` | Brief | 4 | workers |
| `Temp_Worker_Flat_Cost` | Brief | 25,000 | $/worker |
| `Temp_Worker_Hours` | Brief | 1,440 | hrs/worker |
| `Temp_Wage_Per_Hour` | Case materials | 17.36 | $/hr |
| `Tomato_Price` | Case materials | 8,800 | $/bed |
| `Carrot_Price` | Case materials | 2,094 | $/bed |
| `Mesclun_Price` | Case materials | 2,700 | $/bed |
| `Tomato_Fertilizer_Per_Bed` | Case materials | 880 | $/bed |
| `Carrot_Fertilizer_Per_Bed` | Case materials | 440 | $/bed |
| `Mesclun_Fertilizer_Per_Bed` | Case materials | 880 | $/bed |
| `Tomato_Base_Labor_Hrs_Wk_Bed` | Case materials | 2.50 | hrs/wk/bed |
| `Carrot_Base_Labor_Hrs_Wk_Bed` | Case materials | 0.833 | hrs/wk/bed |
| `Mesclun_Base_Labor_Hrs_Wk_Bed` | Case materials | 1.25 | hrs/wk/bed |
| `Tomato_Diminishing_Rate` | Case materials | 10.00% | %/bed |
| `Carrot_Diminishing_Rate` | Case materials | 2.50% | %/bed |
| `Mesclun_Diminishing_Rate` | Case materials | 1.25% | %/bed |

`Farmer_Salary` and `Farmer_Implied_Wage` describe the farmer's own
compensation ($50,000/season, half her time in the field = 720 hours,
implying $34.72/hr across all her working hours — twice the temp wage).
Since this salary is paid regardless of the planting decision, it's a
**second fixed-cost line, separate from `Fixed_Cost`** — not a per-hour
charge against `Own_Labor_Hours` (see §4's marginal-wage logic, unchanged).
`Farmer_Implied_Wage` is **reference-only context** (it's what makes
"temp labor at $17.36/hr is a bargain" legible — half her own rate) — it
does not feed into any formula in §4-§6.

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
- **Marginal wage (shared-pool, step function):** per the brief's assumption
  that "an hour is an hour... the relevant marginal wage is whichever
  source is cheaper at the margin" — evaluated on the **shared, cross-crop
  running total of hours committed so far, *before* adding the bed being
  priced** (not each crop's own cumulative, and not including the bed in
  question):
  `Marginal_Wage(hours_committed_before_this_bed) = IF(hours_committed_before_this_bed < Own_Labor_Hours, 0, Temp_Wage_Per_Hour)`
  For the standalone per-crop crossover (§5), where only one crop is being
  evaluated in isolation, `hours_committed_before_this_bed` is that crop's
  own cumulative hours through bed `n-1`. For the greedy walk and Solver
  scenarios (which allocate across all three crops), it's the running
  `Total_Labor_Hours_Needed` across whichever beds have already been
  committed at that point in the walk/solve — not any single crop's own
  cumulative.
- **Temp-worker hiring cost (lumpy, not per-hour):**
  `Temp_Workers_Hired = CEILING(MAX(0, Total_Labor_Hours_Needed - Own_Labor_Hours) / Temp_Worker_Hours, 1)`, capped at `Temp_Worker_Max`.
  `Temp_Hiring_Cost = Temp_Workers_Hired × Temp_Worker_Flat_Cost` — owed in
  full per worker regardless of hours actually used.
- **Total labor cost (aggregate, not per-crop):**
  `Labor_Cost = Temp_Wage_Per_Hour × MAX(0, Total_Labor_Hours_Needed - Own_Labor_Hours)`
  — the flat per-hour charge on whatever exceeds the free 720 hours; kept
  as one number across all three crops, per the same shared-pool logic as
  `Marginal_Wage` above.
- **Marginal cost per bed:**
  `Marginal_Cost(crop, n) = Marginal_Labor_Hrs(crop, n) × Marginal_Wage(...) + {Crop}_Fertilizer_Per_Bed`

## 5. Marginal Analysis Formulas

Because each crop is sold at a flat, quantity-independent price (price
taker), **marginal revenue per bed = `{Crop}_Price`**, constant regardless
of how many beds of that crop are already planted. So for each crop in
isolation:

- **Crossover bed** = smallest `n` such that `Marginal_Cost(crop, n) > {Crop}_Price`.
- **Standalone optimal beds(crop)** = `crossover bed − 1`, capped at that
  crop's bed cap.

This per-crop crossover is the brief's own back-of-envelope first pass
(see its "What I am assuming" note: reasoning about each crop's curve
"mostly independently for this first pass"). It is **not** the final
answer, because:

1. All three crops draw from one shared, capped labor pool (6,480 hrs max),
   so a bed that clears its own crossover may still be uneconomical if it
   displaces a more valuable bed of another crop.
2. Temp-worker hiring cost is lumpy across the whole allocation, not
   per-crop — the 3rd or 4th worker's $25,000 has to be justified by the
   *marginal* value of the hours it unlocks across all three crops
   combined, not any single crop's curve.

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
optimum) because it is **not guaranteed to match the true joint optimum**:
a bed-by-bed walk that only compares "is this crop's next bed still
profitable?" never explicitly weighs the $25,000 lumpy cost of crossing
into a new temp-worker hour bracket — it only ever "sees" the flat
`Temp_Wage_Per_Hour` rate per hour, never the fixed hiring fee. A greedy
walk can therefore keep adding beds well past the point where the *true*,
fully-costed allocation would stop. Report the greedy walk's resulting
allocation and its total contribution (correctly re-costed with the actual
number of temp workers that allocation requires) alongside the Solver
optimum, so the two can be compared directly.

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
  a single crop under the shared-pool `Marginal_Wage` logic in §4. Labor
  cost and temp-hiring cost are each subtracted once, in aggregate, never
  split across crops.
- Check cell: `Summary` net profit reconciles to
  `SUM(per-crop contribution) − Labor_Cost − Temp_Hiring_Cost − Fixed_Cost − Farmer_Salary`
  computed independently on `Calculations`.

---

## Part B — Analysis Specification

## 7. Analysis Requirements

The current hypothesis (brief frontmatter: "mesclun-max and carrot max mix;
exhaust the highest marginal profit crops up until their limits") is that
**mesclun and carrots each saturate their own bed caps** — mesclun ~45% =
30 beds (its cap), carrots ~30% = 20 beds (its cap) — with tomatoes taking
the remaining ~25% (~10 beds), on the combination of high diminishing
returns, high labor rate, and high fertilizer cost. The stated mechanism is
the P = MC greedy walk described in §5. The analysis must check each of the
brief's five falsification conditions directly against Solver's output (and
against the greedy-walk scenario, since the hypothesis's mechanism and the
Solver optimum are not guaranteed to agree — see §5):

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

Identify which constraint actually binds at the optimum — a crop's own
bed cap, the 6,480-hour labor ceiling, or an unrecovered temp-worker fixed
cost (i.e., hiring the *n*-th worker doesn't pay for itself in added
contribution) — and report the sensitivity of relaxing each binding
constraint by one unit (one more bed of cap, one more labor hour, one more
temp worker). Compute this **manually**: relax the constraint by one unit,
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
  optimum — if they diverge, say by how much and why (the lumpy $25,000
  temp-worker cost is the leading candidate explanation, per §5).
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

---

## 11. Validation Findings (2026-09-09)

Ran the five required checks against the formulas in this spec (§4-§5),
computed directly in Python — not against the workbook, since `model.xlsx`
still reflects the pre-formula-swap spec and hasn't been regenerated yet.
Acceptance criteria: optimal mix Tomatoes 10 / Carrots 20 / Mesclun 30
(60 beds), season profit $42,762, standalone P≈MC points Tomatoes ~10 /
Carrots ~10 / Mesclun ~6.

| # | Check | What I found | What I did |
|---|-------|---------------|------------|
| 1 | q=1 by hand (tomato) | `Cumulative_Labor_Hrs(tomato,1) = 1×2.5×36×1.10 = 99.0` — matches exactly. | **Pass.** No action. |
| 2 | Cross-check against the Farm Profit Lab | I don't have access to the Farm Profit Lab (no URL/credentials/export in this repo or given to me). | **Could not run.** Need the tool's output (or access to it) to compare an intermediate marginal cost. |
| 3 | Two Solver starting points, 0/0/0 and 20/0/0 | Simulated with single-step hill-climbing (since I can't drive Excel Solver's UI directly): **0/0/0 converges to a local optimum at Tomatoes 4/Carrots 0/Mesclun 4, $38,960** — 6.3% below the true global optimum I found by exhaustive search (Tomatoes 6/Carrots 12/Mesclun 13, $41,583). **20/0/0 is infeasible at the start** — 20 tomato beds alone need ~12,110 labor hours, almost double the 6,480-hour max capacity even with all 4 temp workers hired. | **Real finding, not a nuisance, per the check's own framing.** A naive greedy/local search stalls well short of the true optimum, and a plausible-looking starting point (max out the highest-value crop) is infeasible outright. Flagging for the actual Excel Solver run: don't trust a single starting point, and expect GRG Nonlinear to struggle from an infeasible seed. |
| 4 | The check figures | **Fail, on both figures.** Exhaustively searching the full integer decision space under this spec's current cost model (§4: `Marginal_Wage` = $0 up to 720 shared hours, then $17.36/hr; ≤4 temp workers), the true optimum is **Tomatoes 6 / Carrots 12 / Mesclun 13 ($41,583)** — close to the target profit (within 2.8%) but a substantially different allocation (31 beds, not 60). The stated optimal mix, Tomatoes 10/Carrots 20/Mesclun 30, evaluates to **−$12,226 contribution** under this spec — not $42,762. The standalone P≈MC crossovers also don't reproduce ~10/~10/~6: under the spec's current per-crop `Marginal_Wage` rule (§4 — $0 until that crop's *own* cumulative hours pass 720), tomato crosses around bed 11 (close to ~10), but carrot and mesclun never cross within their caps at all (both stay profitable to 20 and 30 beds respectively) — their own hours never reach 720 in isolation. I also tried the crossover with `Temp_Wage_Per_Hour` (17.36) applied uniformly (no 720 threshold) — tomato still ~11, but carrot and mesclun still never cross. I tried `Farmer_Implied_Wage` (34.72) applied uniformly — that produces crossovers at tomato~6, carrot~11, mesclun~7, which is closer for carrot/mesclun but now tomato is off. No single wage assumption I tested reproduces all three target crossovers together. | **Did not patch the spec.** None of the wage-rate variants I tried (the step function as currently specced, uniform temp wage, uniform farmer wage) reproduce the target numbers, and the close-but-not-exact profit match at a very different bed count (31 vs 60) rules out a simple rounding fix — I checked carrot's rounded 0.833 hrs/wk/bed against the unrounded 5/6 and it barely moves the answer. This needs the actual formula/assumption from the Farm Profit Lab or Adam's materials for the standalone and joint cost calculations before I change anything — see open question below. |
| 5 | Formulas, not pasted values | Not yet checkable — `model.xlsx` hasn't been regenerated against the current spec (v1.2), so there's nothing to spot-check yet. | **Deferred** until the workbook is rebuilt, which I'm holding off on until check 4 is resolved (rebuilding now would bake in a cost model that already fails its own acceptance criteria). |

**Bottom line:** Check 1 passes cleanly. Checks 2 and 5 couldn't be run (no
Farm Profit Lab access; no current workbook to inspect). Check 3 surfaced a
real local-optimum and infeasible-start-point risk, independent of the
check-4 question. **Check 4 is the one that matters most, and it fails**:
this spec's cost model (§4's shared-pool `Marginal_Wage` step function)
does not reproduce either the stated optimal mix or the standalone
crossover points, even though it gets the achievable maximum profit into
the right neighborhood. I did not guess at a fix, per the "fix the spec,
don't patch the workbook" rule — patching would mean picking one of the
three wage assumptions I tried above with no real basis for preferring it.

**Open question:** what wage/cost assumption does the Farm Profit Lab (or
Adam's materials) actually use for (a) the standalone per-crop P≈MC
crossover and (b) the joint labor-cost calculation? Once that's confirmed,
I'll update §4 accordingly, re-run all five checks, and only then rebuild
`model.xlsx`.
