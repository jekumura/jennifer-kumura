# Pipeline Projection — spec

**Supports:** `docs/briefs/research-brief.md`, H2 (timing) and "The analysis
I plan to run," item 3. Resolves loose end 4 of the 2026-10-06 instructor
review: "when the cohort model is built, answer in H2 which year the
shortage appears at three years and at five."

**Status:** structure and formulas built; every `Inputs` value is a
placeholder pending Jennifer's actual assumption choices (real hiring
decline, transition rate, attrition rate, base-year mid-level headcount).
Formula correctness was verified against an independent plain-Python
simulation (below); the workbook itself has not been recalculated in Excel
(see "Known limitation" in `README.md`).

## 1. What the model does

Three hiring paths — Baseline, Depressed, Recovery — are tracked for a
single occupation/pipeline, each producing a projected mid-level stock
under two training-lag assumptions (N=3, N=5). The question the model
exists to answer: does a mid-level shortage emerge under Depressed hiring,
does the Recovery scenario's later hiring rebound arrive early enough to
close it, and does the answer change between N=3 and N=5.

## 2. Data inputs (named ranges, `Inputs` tab)

| Named range | Placeholder value | What it is |
|---|---|---|
| `Base_Year` | 2022 | First year of the model; mid-level stock in this year is the seed. |
| `End_Year` | 2035 | Last year modeled. |
| `Baseline_Annual_Hires` | 100 | Juniors hired per year under the pre-2022 trend. **Placeholder.** |
| `Depressed_Hiring_Pct` | 0.70 | Hiring as a share of baseline under Depressed, from `Base_Year`+1 onward. **Placeholder.** |
| `Recovery_Start_Year` | 2028 | Matches the brief: hiring returns to baseline by this year under Recovery. |
| `Transition_Rate` | 0.80 | Share of a hired cohort that survives to become mid-level after N years. **Placeholder.** |
| `Attrition_Rate` | 0.05 | Annual attrition applied to the mid-level stock. **Placeholder.** |
| `N_Primary` | 3 | The brief's primary training-lag assumption. |
| `N_Sensitivity` | 5 | The brief's sensitivity case. |
| `Base_MidLevel_Stock` | 500 | Mid-level headcount anchor in `Base_Year`, shared across all scenarios/N since none have diverged yet. **Placeholder.** |

Values marked **Placeholder** are illustrative round numbers only — they
make the formulas computable, not assumptions Jennifer has decided on or
sourced. Replacing them with real figures is her task, not this build's.

## 3. Derived formulas (`Calculations` tab)

Agreed in the 2026-10-09 research-brief session (see `prompt-log.md`):

```
Hire[t, Baseline]  = Baseline_Annual_Hires
Hire[t, Depressed] = Baseline_Annual_Hires x Depressed_Hiring_Pct   if t >= Base_Year+1, else Baseline_Annual_Hires
Hire[t, Recovery]  = Baseline_Annual_Hires x Depressed_Hiring_Pct   if Base_Year+1 <= t < Recovery_Start_Year
                   = Baseline_Annual_Hires                          if t >= Recovery_Start_Year, else Baseline_Annual_Hires

Graduates[t, s, N] = Hire[t-N, s] x Transition_Rate

MidSupply[Base_Year, s, N] = Base_MidLevel_Stock                    (seed, all scenarios equal)
MidSupply[t, s, N] = (MidSupply[t-1, s, N] + Graduates[t, s, N]) x (1 - Attrition_Rate)   for t > Base_Year

Gap[t, s, N] = MidSupply[t, Baseline, N] - MidSupply[t, s, N]        for s in {Depressed, Recovery}
```

Two design decisions, both Jennifer's:

- **Attrition timing.** Applied *after* the new cohort joins the stock each
  year, not before — a newly-graduated cohort faces attrition risk in its
  first year too. (The alternative — attrition on last year's survivors,
  then add new graduates untouched — was considered and rejected.)
- **Depressed scenario has no built-in recovery.** The brief's "stays at
  the current reduced level through 2030" names the year relevant to H2,
  not an end date for the depression; a self-reverting Depressed scenario
  would collapse into Recovery. Depressed hiring stays reduced for the
  full modeled window (through `End_Year`), not just through 2030.

The `Calculations` tab also carries four shortage-flag columns
(`=IF(Gap>0,1,0)`), one per scenario/N pair, used by `Summary`'s onset-year
lookups so those formulas are plain `MATCH(1, range, 0)` rather than array
formulas.

## 4. Summary tab

For each of the four (scenario, N) pairs — Depressed/N=3, Recovery/N=3,
Depressed/N=5, Recovery/N=5 — reports: onset year (first year Gap>0),
maximum gap, year of maximum gap, gap at `End_Year`, and whether the gap
has closed or still persists at the end of the window.

## 5. Independent verification (pre-Excel)

Before writing the workbook, the same recursion was run as a plain-Python
simulation using the placeholder values above, to catch logic errors
independent of whether Excel/LibreOffice can recalculate the file in this
environment (it currently cannot — see `README.md`). Result:

| | N=3 | N=5 |
|---|---|---|
| Onset year (Depressed) | 2026 | 2028 |
| Max gap (Depressed) | 183.0 | 153.5 |
| Gap at 2035 (Depressed) | 183.0 | 153.5 (still growing, never closes under these placeholders) |
| Onset year (Recovery) | 2026 | 2028 |
| Max gap (Recovery) | 103.2 | 103.2 |
| Gap at 2035 (Recovery) | 79.8 (narrowing) | 88.4 (narrowing) |

This matches the qualitative reasoning from the research-brief conversation
exactly: N=3's onset (2026) is `Base_Year+1+N` = 2023+3; N=5's onset (2028)
is 2023+5 — shorter training lag means the shortage shows up sooner, not
later, under otherwise identical hiring assumptions. The Recovery scenario's
gap visibly narrows relative to Depressed's once hiring rebounds in 2028,
confirming the recovery mechanic is doing something, though under these
illustrative placeholders it narrows without fully closing by 2035 at
either N — a result about this spec's own placeholder values, not a
finding about the real world. **This table is not H2's answer** — it
confirms the formulas behave as designed, using numbers nobody has claimed
are real. Replacing the placeholder inputs with Jennifer's actual chosen
assumptions, then reading the real `Summary` tab, is what actually answers
H2 at both training lags.
