---
type: brief
engagement: perfect-competition
capability: marginal-analysis
date: 2026-08-23
status: committed          # committed | superseded
hypothesis: "Tomato-heavy mix; the labor penalty does not close the revenue gap"
---

# Market Garden Bed Allocation — engagement brief

## The problem

I'm deciding how to fill 64 beds across three crops — tomatoes, carrots, mesclun — for one 36-week season, before anything goes in the ground. It's a one-shot call: no re-planting mid-season if I get it wrong.

I face the market as a price taker — the price per bed for each crop is a flat line I can't move, so there's no way to "sell my way out" of a bad allocation. Decided badly, I either push a crop past the point where its next bed costs more (in labor and fertilizer) than it earns, or I leave beds idle that would still have cleared a profit — either way I'm leaving money on the table against $20,000 of fixed costs that don't care how I planted.

**Fixed:** 64 total beds, per-crop max (20 tomatoes / 20 carrots / 30 mesclun), 36-week season, $20,000 fixed cost, each crop's market price, fertilizer $ per bed, base labor hrs/wk/bed, and how fast that labor requirement grows per additional bed. Also fixed: my own 720 field hours, and the terms for temp labor — up to 4 workers, $25,000 flat each, 1,440 hours each.

**Chosen:** how many beds of each crop to plant, and how many temp workers to hire.

**What limits the choice:** the per-crop bed caps, the 64-bed total, and total available labor hours — 720 (me) plus up to 5,760 (temp workers) = 6,480 max. The temp-worker cost is lumpy: $25,000 is owed the moment I hire one, whether I use 100 of their hours or all 1,440.

## What I am assuming

- The diminishing-returns percentage compounds per additional bed of that crop, applied to labor hours/wk/bed — not to price or fertilizer. Price stays flat because I'm a price taker; what gets harder is working the crop, not selling it.
- An hour is an hour regardless of whose it is — I can route labor to whichever crop needs it, so the relevant marginal wage is whichever source is cheaper at the margin (temp labor at $17.36/hr, once it's in play).
- The $25,000 per temp worker is a fixed hiring cost, not a per-hour one — so the real question is whether total hours needed exceed my own 720, not a marginal price-per-hour question at the first hire.
- I'm reasoning about each crop's own curve mostly independently for this first pass, even though all three draw from the same labor pool. That's the assumption I'd most want to test with more time — it's plausible the binding constraint is total hours, not any single crop's diminishing returns.

## Hypothesis

My hypothesis is that the mesclun would take up the largest number of beds due to its higher price per bed and lowest diminishing returns rate, then carrots, then tomatoes (28 beds of mesclun, 20 beds of carrots, 16 beds of tomatoes). Tomatoes would take the least number of beds because of its high diminishing returns, high labor rates, and high cost of fertilizer per bed.


## How I would know I was wrong
- Carrots end up with more beds than mesclun (even if tomatoes are still last). That would mean mesclun's price-per-bed edge doesn't survive its own diminishing returns curve — i.e., mesclun's marginal return per bed falls off fast enough, early enough, that carrots' flatter (even if lower) curve overtakes it before the bed cap binds. This kills the "mesclun wins because of price + low diminishing returns" claim specifically, separate from whether tomatoes are last.
- Tomatoes stay last even when I equalize fertilizer cost across all three crops. If I zero out the fertilizer-cost gap and tomatoes still land in roughly the same (small) allocation, then fertilizer cost isn't actually doing the explanatory work I credited it with — the diminishing returns curve alone accounts for the gap, and "high fertilizer cost" was padding, not a real driver.
- Tomatoes stay last even when I equalize labor rate across all three crops. Same logic — if flattening the labor-cost difference doesn't move tomatoes off the bottom, labor rate wasn't a load-bearing part of the mechanism either, and the ranking is being driven almost entirely by the diminishing returns curve shape, not the three-factor story I've written.
- Mesclun's marginal per-bed return crosses below carrots' before mesclun reaches [X] beds — pin an actual number here from your curves. Without a specific crossover point, "mesclun wins because lowest diminishing returns" can absorb almost any result. Naming the bed count where you'd expect the crossover to happen (analogous to your 17–18 tomato crossover) is what turns this into something checkable rather than a plausible-sounding story after the fact.
- Total labor hours or total bed count (whichever cap binds first) turns out to reward a more even split across all three crops rather than a lopsided mesclun-heavy one — i.e., moving a bed from mesclun to carrots raises total contribution at the optimum. That would mean the "biggest crop" framing is wrong not because of ranking but because concentration itself is suboptimal, which is a different kind of wrong than getting the order backwards.
