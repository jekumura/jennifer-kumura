---
engagement: perfect-competition
capability: marginal-analysis
date: 2026-09-19
model: capabilities/marginal-analysis/model.xlsx (v1.5)
---

# Market Garden Bed Allocation — analysis

At the Solver optimum (`Solver!C6:C9`, confirmed by the greedy P=MC walk on
`Calculations!A81:I145`), the farm plants **Tomatoes 10 / Carrots 20 /
Mesclun 30** (60 of 64 beds), hires **4 temp workers**, and clears
**$42,761.66** in season profit (`Summary!D19`).

## 1. Why tomatoes stop at ~10 beds when they're the money crop

Tomatoes earn $8,800/bed (`Inputs!C18`) — four times carrots' $2,094 and
more than three times mesclun's $2,700 — and still stop at half their
20-bed cap. Revenue per bed isn't the decision variable; marginal cost at
the margin is. `Calculations!E14` puts tomato bed 10's marginal cost at
$8,248.59; `Calculations!E15` puts bed 11's at $9,390.72. The $8,800 price
sits between them, so bed 10 is the last one that clears its own cost —
matching the mechanism the brief named (P = MC), just resolved crop by
crop rather than assumed (Figure 1).

## 2. Which constraints bind — and what relaxing one is worth

Carrots and mesclun both stop at their bed caps (20 and 30) with marginal
cost still comfortably below price: bed 20 carrot costs $1,688.95 against
a $2,094 price (`Calculations!L24`, $405/bed of margin left on the table);
bed 30 mesclun costs $2,420.10 against $2,700 (`Calculations!S34`, $280/bed
left) — see Figure 2. Economics hasn't ended production for either crop;
a fence has.

Relaxing each cap by one bed and re-solving (holding the other two crops
at their optimal beds, using the model's own cumulative-hours and tiered-
wage formulas — `capabilities/marginal-analysis/spec.md` §4) shows what
that fence costs:

| Constraint relaxed | New total contribution | Shadow price |
|---|---|---|
| `Carrot_Bed_Cap` 20→21 | $63,114.16 | **+$352.49/bed** |
| `Mesclun_Bed_Cap` 30→31 | $63,008.14 | **+$246.47/bed** |

Both relaxations stay well inside the other constraints (61 of 64 total
beds; ~5,352–5,368 of 6,480 labor-hours) — real, actionable shadow prices,
not artifacts of hitting a second limit. They say what an extra bed of
ground is worth for each crop, and in which order to buy it.

Two constraints carry slack instead: total beds (60 used of 64,
`Summary!B9`) and temp workers (4 hired of a 4-worker max, `Summary!D12`) —
but the worker count isn't actually capped by `Temp_Worker_Max`; it's
`CEILING`'d from the 5,277.22 hours actually needed (`Summary!D13`,
`Calculations!E48` = 3.16 unrounded), well under the 6,480-hour ceiling.
Neither is worth paying to relax.

## 3. The tomato MC dip at ~6 beds

Tomato MC drops at bed 6 before increasing again. This appears to be
caused by the labor-cost structure: the first 720 labor hours are priced
at the farmer's higher implied wage of $34.72/hour, while hours beyond
720 are priced at the lower temporary-worker rate of $17.36/hour. In the
Calculations tab, cumulative tomato labor reaches 724.73 hours at bed 5,
meaning the 720-hour threshold has already been crossed by bed 5. By bed
6, cumulative labor reaches 956.64 hours, so the additional labor for bed
6 falls entirely into the cheaper temporary-worker tier. This lowers the
marginal cost of bed 6, creating the temporary dip before diminishing
returns cause marginal cost to increase again (Figure 1).

## 4. Why grow crops that lose money on their own

Priced alone against the full $20,000 fixed cost, carrots lose money at
every quantity from 1 to their 20-bed cap (best case, all 20 beds: **−$16,489**),
and mesclun likewise at every quantity to 30 beds (best case: **−$11,922**).
Yet the joint optimum plants both to their caps. (Tomatoes are the
exception here — run alone, tomato's own best standalone quantity, 10
beds, actually nets **+$6,173**; it's the one crop that would pencil out
even carrying the whole fixed cost by itself. The "loses money alone"
story belongs to carrots and mesclun specifically, not to all three
crops.)

The resolution is MC vs. AVC, not MC vs. price-including-fixed-cost.
Fixed costs are paid whether or not a bed is planted, so they have no
place in the planting decision. Average variable cost (fertilizer +
labor, no fixed cost) stays below price for carrots and mesclun at every
bed count checked (e.g. carrot AVC at 20 beds: $1,918.45 vs. $2,094
price; mesclun AVC at 30 beds: $2,430.74 vs. $2,700 price) — every bed
contributes something toward the $20,000 the farm owes regardless. Each
crop "loses money" standalone only because it's being asked to carry that
whole fixed cost alone. This is the short-run shutdown rule in the field,
and it's the same reasoning that keeps an airline flying a half-empty
route: price above average variable cost means fly it anyway.

## Against the Stage 1 hypothesis

I predicted a mesclun-max / carrot-max mix (~45%/30%/25% mesclun-carrot-
tomato) via a P=MC greedy walk, and named diminishing-returns rate as the
reason mesclun would out-plant carrots. The model landed on exactly that
allocation — Tomatoes 10 / Carrots 20 / Mesclun 30 (`Summary!D27`:
carrots did not out-plant mesclun) — so the headline numbers held.

But the mechanism I gave was wrong. I treated each crop's own diminishing-
returns curve as what would decide how far it got planted. It doesn't:
mesclun's own standalone P=MC crossover is around bed 6–7
(`capabilities/marginal-analysis/spec.md` §6), nowhere near its 30-bed
cap — and §2 above shows both capped crops still have $250–400/bed of
margin left when the caps stop them. What actually lets mesclun (and
carrot) reach their caps is the order beds get planted in: because
tomatoes absorb the farmer's expensive 720-hour tier first, carrots and
mesclun enter the shared labor pool already past that threshold and pay
the cheap temp wage from their own bed 1 (greedy walk, `Calculations!A81:I145`
— carrot bed 1 costs $973.85 in the joint walk vs. $1,507.71 priced
standalone, `Calculations!L5`). My hypothesis got the destination right and the
route wrong: it isn't "mesclun's curve is flattest, so it wins on its own
terms" — it's "the bed caps bind well before any crop's own economics
would have stopped it, and which crop gets the cheap labor tier depends
on planting order across the whole shared pool, not any one crop's rate."
A model built only on standalone per-crop curves — which is what the
hypothesis's reasoning implicitly assumed — would have badly undersold
mesclun and carrot's real profitability.

## Figures

- `figures/tomato-mc-vs-price.png` — tomato marginal cost vs. $8,800
  price, beds 1–20: the wage-tier dip at bed 6 and the P=MC crossover
  between beds 10 and 11.
- `figures/carrot-mesclun-mc-vs-price.png` — carrot and mesclun marginal
  cost (joint allocation) vs. their prices, showing both curves still
  under price when their bed caps stop them.
