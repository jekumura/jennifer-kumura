# Pipeline Projection — spec

**Supports:** `docs/briefs/research-brief.md`, H2 (timing) and "The analysis
I plan to run," item 3. Resolves loose end 4 of the 2026-10-06 instructor
review: "when the cohort model is built, answer in H2 which year the
shortage appears at three years and at five."

**Status:** structure and formulas built. Four of five Inputs values are now
real or confirmed: `Baseline_Annual_Hires` and `Depressed_Hiring_Pct` are
sourced from BLS and the primary paper, `Transition_Rate` and
`Attrition_Rate` are Jennifer's confirmed judgment calls (see §2 below for
sourcing on each). `Base_MidLevel_Stock` is still a placeholder — Jennifer
is holding it there until the IPUMS extract gives a real mid-level
headcount for Graphic Designers, rather than guessing. Formula correctness
was verified against an independent plain-Python simulation (below); the
workbook itself has not been recalculated in Excel (see "Known limitation"
in `README.md`).

## 1. What the model does

Three hiring paths — Baseline, Depressed, Recovery — are tracked for a
single occupation/pipeline, each producing a projected mid-level stock
under two training-lag assumptions (N=3, N=5). The question the model
exists to answer: does a mid-level shortage emerge under Depressed hiring,
does the Recovery scenario's later hiring rebound arrive early enough to
close it, and does the answer change between N=3 and N=5.

## 2. Data inputs (named ranges, `Inputs` tab)

| Named range | Value | What it is |
|---|---|---|
| `Base_Year` | 2022 | First year of the model; mid-level stock in this year is the seed. |
| `End_Year` | 2035 | Last year modeled. |
| `Baseline_Annual_Hires` | 20,000 | BLS Occupational Outlook Handbook, Graphic Designers (2024–2034 projections): ~20,000 average annual openings. Bundles growth and replacement openings — not specifically junior hires. |
| `Depressed_Hiring_Pct` | 0.855 | 1 − 0.145. Brynjolfsson, Chandar & Chen (revised Aug 2026), Table 1 Panel A: AI-exposure Quintile 4 (Graphic Designers' quintile per Table A.5) shows a −14.5% employment coefficient for ages 22–25 vs. Quintile 1, primary 2018-balanced sample. A regression coefficient on employment stock, not a literal hiring-rate reduction — used as a proxy per the paper's Fact 4 (decline operates through reduced hiring, not separations). |
| `Recovery_Start_Year` | 2028 | Matches the brief: hiring returns to baseline by this year under Recovery. |
| `Transition_Rate` | 0.80 | Jennifer's professional judgment. No occupation-specific source found. |
| `Attrition_Rate` | 0.05 | Jennifer's professional judgment. No occupation-specific source found. |
| `N_Primary` | 3 | The brief's primary training-lag assumption. |
| `N_Sensitivity` | 5 | The brief's sensitivity case. |
| `Base_MidLevel_Stock` | 500 | **Still a placeholder.** Mid-level headcount anchor in `Base_Year`. Held here deliberately pending the real IPUMS extract for Graphic Designers, rather than guessed. |

Four of the five values above are now real or confirmed; only
`Base_MidLevel_Stock` remains an illustrative round number pending the
IPUMS extract.

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
simulation, to catch logic errors independent of whether Excel/LibreOffice
can recalculate the file in this environment (it currently cannot — see
`README.md`). Two passes:

**Pass 1 (illustrative placeholders, before real values were sourced):**

| | N=3 | N=5 |
|---|---|---|
| Onset year (Depressed) | 2026 | 2028 |
| Max gap (Depressed) | 183.0 | 153.5 |
| Onset year (Recovery) | 2026 | 2028 |
| Max gap (Recovery) | 103.2 | 103.2 |

**Pass 2 (re-run with the four real/confirmed values — `Baseline_Annual_Hires`
20,000; `Depressed_Hiring_Pct` 0.855; `Transition_Rate` 0.80; `Attrition_Rate`
0.05 — while `Base_MidLevel_Stock` stays at its placeholder, 500):**

| | N=3 | N=5 |
|---|---|---|
| Onset year (Depressed) | 2026 | 2028 |
| Max gap (Depressed) | ~17,688 | ~14,836 |
| Baseline stock at 2035 | ~148,201 | ~148,201 |
| Onset year (Recovery) | 2026 | 2028 |
| Max gap (Recovery) | ~9,972 | ~9,972 |

Both passes confirm the same qualitative pattern: N=3's onset (2026) is
`Base_Year+1+N` = 2023+3; N=5's onset (2028) is 2023+5 — shorter training
lag means the shortage shows up sooner, not later, under otherwise
identical hiring assumptions. That onset-timing result is now meaningful,
since it comes from real inputs except the stock anchor, which doesn't
affect *when* the gap turns positive (only its size).

**The magnitude numbers in Pass 2 are not meaningful, and that's worth
flagging plainly.** With a real 20,000/year hiring flow compounding against
the still-placeholder 500-person starting stock, the baseline stock balloons
to ~148,000 by 2035 — an obvious scale artifact of mixing a real flow with
a placeholder stock, not a real finding about the Graphic Designer pipeline.
This is itself useful confirmation that `Base_MidLevel_Stock` isn't optional
to leave blank indefinitely: once it's real, Pass 2's gap magnitudes will
mean something; until then, only the onset-year pattern should be read as
informative. **Neither table is H2's final answer** — replacing the last
placeholder with a real mid-level headcount for Graphic Designers, then
reading the actual `Summary` tab, is what answers H2 at both training lags.
