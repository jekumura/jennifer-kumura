---
type: brief
engagement: perfect-competition
capability: marginal-analysis
date: 2026-08-24
status: committed          # committed | superseded
hypothesis: "Tomato-heavy mix; the labor penalty does not close the revenue gap"
---

# Market Garden Bed Allocation — engagement brief

## The problem

I'm deciding how to fill 64 beds across three crops — tomatoes, carrots, mesclun — for one 36-week season, before anything goes in the ground. It's a one-shot call: no re-planting mid-season if I get it wrong.

I face the market as a price taker — the price per bed for each crop is a flat line I can't move, so there's no way to "sell my way out" of a bad allocation. Decided badly, I either push a crop past the point where its next bed costs more (in labor and fertilizer) than it earns, or I leave beds idle that would still have cleared a profit — either way I'm leaving money on the table against $20,000 of fixed costs that don't care how I planted.

**Fixed:** 64 total beds, per-crop caps (20 tomatoes / 20 carrots / 30 mesclun), 36-week season, $20,000 fixed cost, each crop's market price, fertilizer $/bed, base labor hrs/wk/bed, and how fast that labor requirement grows per additional bed. Also fixed: my own 720 field hours, and the terms for temp labor — up to 4 workers, $25,000 flat each, 1,440 hours each.

**Chosen:** how many beds of each crop to plant, and how many temp workers to hire.

**What limits the choice:** the per-crop bed caps, the 64-bed total, and total available labor hours — 720 (me) plus up to 5,760 (temp workers) = 6,480 max. The temp-worker cost is lumpy: $25,000 is owed the moment I hire one, whether I use 100 of their hours or all 1,440.

## What I am assuming

- The diminishing-returns percentage compounds per additional bed of that crop, applied to labor hours/wk/bed — not to price or fertilizer. Price stays flat because I'm a price taker; what gets harder is working the crop, not selling it.
- An hour is an hour regardless of whose it is — I can route labor to whichever crop needs it, so the relevant marginal wage is whichever source is cheaper at the margin (temp labor at $17.36/hr, once it's in play).
- The $25,000 per temp worker is a fixed hiring cost, not a per-hour one — so the real question is whether total hours needed exceed my own 720, not a marginal price-per-hour question at the first hire.
- I'm reasoning about each crop's own curve mostly independently for this first pass, even though all three draw from the same labor pool. That's the assumption I'd most want to test with more time — it's plausible the binding constraint is total hours, not any single crop's diminishing returns.

## Hypothesis

I expect the optimal mix to sit tomato-heavy, close to its 20-bed cap, because tomatoes start with a per-labor-hour advantage large enough that the diminishing-returns penalty doesn't catch up before the cap does.

At bed one, before any penalty: tomatoes clear roughly $70/labor-hour in contribution (90 season-hours/bed, ~$6,358 contribution/bed at temp wages), against roughly $38/hour for carrots and $23/hour for mesclun. That's a 2–3x head start.

Tomatoes also carry the steepest penalty — 10%/bed versus 2.5% for carrots and 1.25% for mesclun — so this is the real test: does the fastest-growing penalty erase the biggest advantage before the cap binds? Working the compounding forward, tomatoes' marginal cost (labor + fertilizer) doesn't cross the $8,800 price until somewhere around the 17th–18th bed of its 20-bed cap. If that's roughly right, most of the tomato allocation stays worth planting — the penalty bites late, not early.

## How I would know I was wrong

- The optimizer lands on materially fewer than ~15 tomato beds (say, under 10). That would mean the penalty bites much earlier than my back-of-envelope crossover around bed 17–18, and the head start doesn't survive it.
- Total labor hours (6,480 cap), not tomatoes' own diminishing returns, turns out to be the binding constraint — i.e., moving an hour off tomatoes onto carrots or mesclun raises total contribution. That would mean hours are worth more spread out than concentrated, which is the opposite of "tomato-heavy."
- The $25,000 fixed cost of a 3rd or 4th temp worker isn't recovered by the extra tomato hours it buys — meaning the honest optimum caps out on fewer hired workers, which caps tomatoes below where I'm expecting.
