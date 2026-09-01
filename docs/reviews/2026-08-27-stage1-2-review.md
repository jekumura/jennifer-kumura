<!-- PR TARGET: https://github.com/jekumura/jennifer-kumura | Stage 1.2 (8 pts) -->
# Stage 1.2 review — spec, build, audit

**You scored 50 out of 100 (F), which would be 7.50 of the 15 points for this stage. I am holding it rather than entering it — see below.**

**Spec:** [`capabilities/marginal-analysis/spec.md`](https://github.com/jekumura/jennifer-kumura/blob/main/capabilities/marginal-analysis/spec.md)

> Graded early. Stage 1.2 is not due until 6 September and nothing was required yet — you are one of only two people in the cohort with a spec and a working workbook, so this is formative feedback well ahead of the deadline. I am not entering this score. There are two costing defects in the workbook, and once they are fixed the model should land somewhere very different. Fix them before the deadline and the stage is re-graded from scratch.

| Criterion | Earned | Notes |
|---|---|---|
| Spec completeness — inputs, structure, calculation flow | 24.0 / 37.5 | The architecture is genuinely good. Ten numbered sections, a clean split between Part A (model) and Part B (analysis), every input given a named range with a unit, and a Part B that specifies the analysis before it exists. The workbook that came out of it has 29 defined names, all absolute. Two things cost you here. Your labor function is specified as marginal hours per bed compounding on the bed number — base x 36 x (1 + rate)^(n-1), summed down the column — but the case defines total hours for q beds as q x base x 36 x (1 + rate)^q, where each additional bed makes every bed of that crop more labor-hungry, not just the newest one. At 10 tomato beds yours gives 1,434 hours where the case gives 2,334. And Carrot_Base_Labor_Hrs_Wk_Bed is entered as the displayed 0.833 rather than derived as tomato hours divided by three. |
| Spec validation rules | 8.0 / 25.0 | Section 6 lists the structural constraints — beds within caps, total beds within 64, labor covered by the hours purchased — plus a reconciliation check between the Summary and Calculations sheets. Those are real, and they all pass. What is missing is the part this criterion is mostly about: the published check figures, written down as acceptance criteria before the build. There is no 10 / 20 / 30, no $42,762, no q = 1 hand check, and no tolerance table. That absence is why the two defects below could sit in the workbook without anything going red. |
| Workbook satisfies the contract | 10.0 / 25.0 | Structurally the file is sound: 484 formulas, zero error cells across five sheets, 29 absolute named ranges, live constraint checks that all read OK, and a Summary panel that honestly reports its own hypothesis verdict as FAILS. But the contract is that Solver reproduces 10 / 20 / 30 at about $42,762, and yours returns 10 / 15 / 4 at negative $8,630 — a gap of roughly $51,000. The two defects below account for most of it. |
| Audit note | 8.0 / 12.5 | Your Notes tab does real work: data sources with provenance, the key assumptions carried across from the brief, how the optimum was found, how to re-run Solver, and written procedures for the three sensitivity checks you have not automated. Reporting your own hypothesis check as FAILS on the Summary sheet is exactly the right instinct. What is missing is a defects-found-and-fixed record — the stage asks for at least three checks, each naming what it would have caught, and the two errors below went uncaught. |
| **Final** | **50 / 100** | **held — do not enter** |

### The two defects, and how to find them yourself

I would rather you re-derive these than take my word for them, so here is what to check.

- Temporary labor is charged twice. Your Summary reports Labor cost $24,920.52 and, separately, Temp-worker hiring cost $25,000. Look at what each one is: 1,435.51 temp hours x $17.36 = $24,920, and the $25,000 flat fee buys 1,440 hours at that same $17.36. They are the same money. Your own Notes tab says the hiring cost is "a lumpy fixed cost per worker, not a per-hour charge" — so the per-hour line should not also be there. The model contradicts its own spec, which is the single most useful kind of error to find, because the spec tells you which one is wrong.

- The diminishing-returns exponent is applied to the wrong thing. In Calculations you compute marginal hours for bed n as base x 36 x (1 + rate)^(n-1) and then sum down the column. That treats the penalty as applying only to the newest bed. The case applies it to the whole crop: total hours for q beds = q x base x 36 x (1 + rate)^q — "each extra bed makes every bed a little more labor-hungry." Test it at one point: your tomato cumulative at 10 beds is 1,434.37 hours; the case's formula gives 2,334.37. That is a 900-hour difference on one crop, and it is why marginal cost rises too slowly in your schedule.

- One input to tighten while you are in there: Carrot_Base_Labor_Hrs_Wk_Bed is 0.833, but the case defines carrot labor as tomato labor divided by three, which is 0.8333 recurring. Enter it as a formula rather than the rounded display value. Small on its own, and it is the same class of error as the two above — a displayed number treated as an exact input.

### The one that is a judgment call, not a defect

You add the farmer's full $50,000 salary as a fixed cost on top of the $20,000, and treat her 720 field hours as free at the margin because they are already paid for. That is a defensible reading and you argued it explicitly, which I respect — it is the accounting-versus-economic-cost question, and it is one of the things this case is built to surface.

It is not the convention the case uses, though. The case charges the farmer's 720 field hours into labor at her implied $34.72 rate and leaves the rest of her salary out of the model entirely; the $20,000 is the only fixed cost. Because the published check figures are computed that way, a model built on your convention cannot hit them no matter how correct it is internally.

My advice: build to the case convention so the check figures are reachable, and keep your argument. Note in the spec that you would treat the farmer's salary differently, why, and what it does to the answer. That is a stronger position than either choice alone, and it is exactly what the Stage 3 decision memo is for.

### What I'd do in order

- Add the check figures to Section 6 first, before touching the model: 10 / 20 / 30 beds, profit about $42,762, roughly 3.16 temporary workers, standalone crossings near 10 / 10 / 6, and tomato labor of exactly 99 hours at q = 1. Give each one a tolerance. Do this first, because it turns the next three fixes from guesswork into something with a pass light.

- Fix the labor function. One formula, three crops.

- Remove the double charge on temporary labor — decide whether a worker costs $25,000 as a lump or $17.36 an hour, and charge it once.

- Re-run and read the q = 1 check. Tomato labor should come out at 99.00 hours exactly. If it does not, the labor function is still wrong and nothing downstream is worth reading yet.

- Then record what you found in the spec: what you checked, what it would have caught, what you fixed. That is the Audit note criterion, and you will have earned it honestly by then.

### One thing worth saying

Building the Summary panel so it reports its own hypothesis verdict — and letting it print FAILS rather than quietly adjusting the hypothesis to match — is a better instinct than most of what I graded this week. The defects above are ordinary modeling errors that everyone makes; the habit of building something that can tell you that you were wrong is the rarer thing, and you already have it.

Your docs/standards/excel-formatting.md, written before anyone asked for it, is what the rest of the cohort will wish they had in two weeks.

### A note on the point value, new as of today

This stage is now worth **15 points** rather than the 8 in the stage brief, and **Stage 1.3** — the analysis, the memo, and the prompt log — is now worth **15** as well. Cases 2 and 3 have been dropped for this cohort, so Case 1 *is* the case.

In practice: this stage and the next one are together worth **30 of the 35 points** on the case. Stage 0 and Stage 1.1 are 2.5 each. The weight has moved onto the build and the analysis, which is where the work actually is.

Nothing about the grading changes — the score is still out of 100 and converted at the end. The stage brief and the case page still show the old numbers; they have not been updated yet.

---

### How to work this review

Treat this PR the way an analyst treats feedback from a senior reviewer — a review is a proposal to engage with, not a checklist to rubber-stamp.

1. **Read it yourself first.** Form your own view before you change anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM.** Paste this review and your spec into your assistant and ask it to (a) explain anything you are unsure of, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change.
3. **Then correct the spec, not the workbook.** This is the rule that makes the stage work: when a check fails, you fix the specification and regenerate, so the document keeps describing what was actually built.
4. **Close the loop.** Reply in this thread with what you changed and what you pushed back on, then commit and push.

*Nothing here is final. Stage 1.2 is not due until 6 September, and the stage is re-graded from scratch at the deadline.*

— Adam
