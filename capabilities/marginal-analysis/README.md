# Marginal Analysis

## What it is

A method for allocation decisions where the choice is "how many units of
each option, given diminishing returns and a resource everything draws
from." The core move: find where marginal cost meets marginal revenue
(P = MC) for each option, then solve the whole allocation jointly rather
than optimizing each option in isolation — a shared, constrained resource
(here, labor hours split between an owner's own time and hired help) means
one option's marginal cost depends on how much of the shared resource the
others have already used.

The build (`spec.md`, `model.xlsx`) follows this repo's standard shape:
named-range inputs, a per-option marginal-cost curve, a joint optimum
solved with Excel Solver, and a cheaper "greedy" reference calculation
(assign the next unit to whichever option currently has the highest
marginal profit) to sanity-check Solver's answer against intuition.

## Where it was exercised

- **perfect-competition** — a price-taking market garden deciding how many
  of 64 beds to plant with tomatoes, carrots, and mesclun, and how many
  temp workers to hire, under compounding diminishing labor returns and a
  shared labor pool. [Brief](../../docs/briefs/perfect-competition-brief.md) ·
  [Spec](./spec.md) · [Model](./model.xlsx)
