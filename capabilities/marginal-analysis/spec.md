---
type: spec
capability: marginal-analysis
engagement: perfect-competition
date: 2026-08-24
version: 1.0
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
the $20,000 fixed cost. Audience: the grower making the planting call.

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
- **`Solver`** — decision variables (beds per crop, temp workers hired) and
  constraint cells, set up for Excel Solver (this is a constrained,
  non-linear, mixed-integer problem — not a closed-form calculation, so the
  optimum can't just be read off three independent marginal-cost curves).
- **`Notes`** — model description, assumptions from the brief (§ "What I am
  assuming"), data sources, date built.

## 3. Data Inputs

Values the brief states explicitly are filled in. Values the brief
identifies as fixed/known but doesn't state a number for are left blank —
pull those from the case materials, don't infer them.

| Named Range | Source | Value | Unit |
|-------------|--------|-------|------|
| `Total_Beds` | Brief | 64 | beds |
| `Tomato_Bed_Cap` | Brief | 20 | beds |
| `Carrot_Bed_Cap` | Brief | 20 | beds |
| `Mesclun_Bed_Cap` | Brief | 30 | beds |
| `Season_Weeks` | Brief | 36 | weeks |
| `Fixed_Cost` | Brief | 20,000 | $ |
| `Own_Labor_Hours` | Brief | 720 | hrs |
| `Temp_Worker_Max` | Brief | 4 | workers |
| `Temp_Worker_Flat_Cost` | Brief | 25,000 | $/worker |
| `Temp_Worker_Hours` | Brief | 1,440 | hrs/worker |
| `Temp_Wage_Per_Hour` | Brief | 17.36 | $/hr |
| `Tomato_Price` / `Carrot_Price` / `Mesclun_Price` | Case materials | *(blank)* | $/bed |
| `Tomato_Fertilizer_Per_Bed` / `Carrot_Fertilizer_Per_Bed` / `Mesclun_Fertilizer_Per_Bed` | Case materials | *(blank)* | $/bed |
| `Tomato_Base_Labor_Hrs_Wk_Bed` / `Carrot_Base_Labor_Hrs_Wk_Bed` / `Mesclun_Base_Labor_Hrs_Wk_Bed` | Case materials | *(blank)* | hrs/wk/bed |
| `Tomato_Diminishing_Rate` / `Carrot_Diminishing_Rate` / `Mesclun_Diminishing_Rate` | Case materials | *(blank)* | %/bed |

## 4. Derived Inputs

- **Per-bed labor hours (compounding diminishing returns):**
  `Marginal_Labor_Hrs(crop, n) = {Crop}_Base_Labor_Hrs_Wk_Bed × Season_Weeks × (1 + {Crop}_Diminishing_Rate)^(n-1)`
  — applied to labor hours only, per the brief's assumption; price and
  fertilizer per bed stay flat regardless of bed count.
- **Cumulative labor hours for a crop at N beds:**
  `Cumulative_Labor_Hrs(crop, N) = SUM(Marginal_Labor_Hrs(crop, 1..N))`
- **Marginal wage (shared-pool, step function):** per the brief's assumption
  that "an hour is an hour... the relevant marginal wage is whichever
  source is cheaper at the margin" —
  `Marginal_Wage(cumulative_hours_used) = IF(cumulative_hours_used <= Own_Labor_Hours, 0, Temp_Wage_Per_Hour)`
- **Temp-worker hiring cost (lumpy, not per-hour):**
  `Temp_Workers_Hired = CEILING(MAX(0, Total_Labor_Hours_Needed - Own_Labor_Hours) / Temp_Worker_Hours, 1)`, capped at `Temp_Worker_Max`.
  `Temp_Hiring_Cost = Temp_Workers_Hired × Temp_Worker_Flat_Cost` — owed in
  full per worker regardless of hours actually used.
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

The actual optimum requires jointly solving beds-per-crop and
temp-workers-hired via the `Solver` tab (integer/non-linear constraints:
per-crop bed caps, total bed cap, total labor hours, integer 0–4 temp
workers), maximizing total contribution. The per-crop crossover formulas
above exist to sanity-check Solver's output against intuition, not to
replace it.

## 6. Validation Rules

- `SUM(beds per crop) <= Total_Beds`
- `beds(crop) <= {Crop}_Bed_Cap` for each crop
- `Temp_Workers_Hired` is a non-negative integer `<= Temp_Worker_Max`
- `Total_Labor_Hours_Needed <= Own_Labor_Hours + Temp_Workers_Hired × Temp_Worker_Hours`
- Check cell: `Summary` net profit reconciles to
  `SUM(crop contribution) − Temp_Hiring_Cost − Fixed_Cost` computed
  independently on `Calculations`.

---

## Part B — Analysis Specification

## 7. Analysis Requirements

The current hypothesis (brief, "Hypothesis" section) is that the optimal
mix ranks **mesclun > carrots > tomatoes** by bed count — mesclun largest
on higher price/bed plus the lowest diminishing-returns rate, tomatoes
smallest on the combination of high diminishing returns, high labor rate,
and high fertilizer cost. The analysis must check each of the brief's five
falsification conditions directly against Solver's output:

1. Does carrots end up with **more** beds than mesclun? If so, mesclun's
   price + low-diminishing-returns edge doesn't survive its own curve.
2. Zero out the fertilizer-cost gap across crops and re-solve — do tomatoes
   still land last? If yes, fertilizer cost wasn't doing real explanatory
   work.
3. Zero out the labor-rate gap across crops and re-solve — do tomatoes
   still land last? If yes, labor rate wasn't load-bearing either, and the
   ranking is driven by the diminishing-returns curve shape alone.
4. At what bed count does mesclun's marginal per-bed return cross *below*
   carrots'? Report the actual number (analogous to a tomato-style
   crossover bed) rather than leaving the claim unfalsifiable.
5. At the optimum, does moving one bed from mesclun to carrots raise total
   contribution? If so, the binding constraint (labor hours or total beds)
   rewards a more even split, not mesclun-heavy concentration.

## 8. Constraint Diagnosis

Identify which constraint actually binds at the optimum — a crop's own
bed cap, the 6,480-hour labor ceiling, or an unrecovered temp-worker fixed
cost (i.e., hiring the *n*-th worker doesn't pay for itself in added
contribution) — and report the shadow-price-style sensitivity of relaxing
each binding constraint by one unit (one more bed of cap, one more labor
hour, one more temp worker).

## 9. Strategic Recommendations

- Final beds-per-crop allocation and temp-worker hiring decision.
- Explicit verdict on the hypothesis: confirmed, or which of the five
  falsification conditions in §7 tripped and what that implies about the
  "mesclun wins on price + low diminishing returns, tomatoes lose on all
  three factors" story.
- Sensitivity notes from §8 (what changes the recommendation if a
  constraint were relaxed).

## 10. Output Format

1. Allocation table (beds per crop, temp workers hired).
2. Total contribution and net profit.
3. Hypothesis verdict against each of the five §7 checks.
4. Binding constraint and its sensitivity (§8).

---

## References

- [`docs/briefs/perfect-competition-brief.md`](../../docs/briefs/perfect-competition-brief.md) — engagement brief (problem framing, fixed/chosen variables, assumptions, hypothesis, falsification criteria)
- [`docs/standards/excel-formatting.md`](../../docs/standards/excel-formatting.md) — workbook build conventions
