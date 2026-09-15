@jekumura

Reviewed below, criterion by criterion. This is entered.

CRITERION BY CRITERION

* **Spec completeness — inputs, structure, calculation flow** — **v1.5 derives all three of the case's rounded displays and states the general rule behind them.**  The architecture was always good — ten numbered sections, a clean split between Part A (model) and Part B (analysis), every input given a named range with a unit, and a Part B that specifies the analysis before it exists. What was wrong is now fixed. Your labor function had been specified as marginal hours compounding on the bed number, summed down the column, where the case defines total hours for q beds as q × base × 36 × (1 + rate)^q — every bed of that crop gets more labor-hungry, not just the newest. At 10 tomato beds yours gave 1,434 hours against the case's 2,334. You replaced it with the case's closed form. `Carrot_Base_Labor_Hrs_Wk_Bed` is now `=Tomato_Base_Labor_Hrs_Wk_Bed/3` rather than a typed 0.833.
* **Spec validation rules** — This was the criterion carrying the whole gap and it is now the strongest part of the recovery. The published check figures are written down as acceptance criteria, and — the part that earns the marks — your defect record names, for each defect, **the check that caught it**, and where nothing caught it, says "caught by: nothing at the time." Your note explaining why the q = 1 hand check *could not* have found the compounding bug, because both formulas agree trivially at q = 1, is the sharpest sentence anyone wrote on this stage. The self-invented "±$50 tolerance" on check 4 is gone, replaced by ±$0.01 with a written justification for why no other slack is defensible. You also widened check 5 to cover the `Inputs` sheet — the change that would have caught the defect below.
* **Workbook satisfies the contract** — The rebuild lands. Solver returns 10 / 20 / 30, against the 10 / 15 / 4 at −$8,630 it returned before — a swing of about $51,000, and the whole of it traceable to the two costing defects you found and removed. Season profit read $42,768.33 against the published $42,761.66; that residual is now **closed**. The v1.5 rebuild returns $42,761.664682745 — exact, against a target that now needs no tolerance at all.
* **Audit note** — Near full marks. The defects-found-and-fixed record that was missing now exists, each entry naming what was checked, what was found, and what was done. Reporting your own hypothesis verdict as FAILS on the Summary sheet before you had fixed anything was always the right instinct; the record now completes it.

**YOU CLOSED THE $6.67, AND THEN YOU FIXED THE CHECK THAT LET IT THROUGH**

The residual is gone. `Farmer_Implied_Wage` and `Temp_Wage_Per_Hour` were `34.72` and `17.36`, typed
in; they are now `=Farmer_Season_Salary/2/Own_Labor_Hours` and
`=Temp_Worker_Flat_Cost/Temp_Worker_Hours`. Profit moves from $42,768.33 to **$42,761.664682745**,
which is my figure to every digit I carry.

That is the expected half. The part that earns the last points is the other half.

You did not only record the defect — you recorded **why nothing caught it**: check 5 scanned
`Calculations`, both literals lived on `Inputs`, so the check could not have found them however
carefully it was run. Then you widened the check's scope so that it would.

Fixing a defect is ordinary. Fixing the control that should have caught the defect, and saying plainly
that it could not have, is what an audit is for. You have now done that twice on this stage.

**THE RULE YOU WROTE IS THE GENERALIZATION, AND IT IS CORRECT**

> any input that is itself a computed ratio … is entered as a formula referencing its component named
> ranges — never as a typed decimal. The case materials' printed number … describes that ratio for a
> human reader, rounded for display; it is not the value to compute with.

That is the whole of this defect class, stated once, where a future builder will read it before making
the same mistake. It generalizes well past this case.

**I RE-VERIFIED YOUR CROSS-CHECK AND IT IS EXACT**

Your Farm Profit Lab comparison reports bed-1 marginal contributions. I recomputed all three
independently against my own model:

| Crop | Yours | Mine |
|---|---|---|
| Tomato bed 1 | $4,482.50 | $4,482.50 |
| Carrot bed 1 | $586.29 | $586.29 |
| Mesclun bed 1 | $237.97 | $237.97 |

Exact, all three. And noting that the lab displays whole dollars while your model carries cents is the
right way to describe an agreement instead of overclaiming it.

Your reading of the 20/0/0 start is right as well: 20 tomato beds alone need about 12,110 hours
against a 6,480-hour ceiling, so that start is infeasible outright rather than merely path-dependent.

**WHERE THIS LEAVES YOU**

This stage opened held and held. It closes at full marks, and the reason is not that the
arithmetic got fixed — it is that each time something was wrong, you went and found the control that
should have caught it. That habit is the part that transfers.

---

**How to reply to this review.** Comment on this pull request with what you changed, or push another
commit to `main` and say so here. If you disagree with something, say that too — a disagreement you
can support is worth more to me than a correction you make because I asked. This stage is still open.

