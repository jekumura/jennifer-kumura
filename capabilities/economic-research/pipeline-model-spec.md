# Pipeline Projection — spec

**Supports:** `docs/briefs/2026-10-09-research-brief.md`, H2 (timing) and "The analysis
I plan to run," item 3. Resolves loose end 4 of the 2026-10-06 instructor
review: "when the cohort model is built, answer in H2 which year the
shortage appears at three years and at five."

**Status:** structure and formulas built. All five Inputs values are now
real or confirmed: `Baseline_Annual_Hires` and `Depressed_Hiring_Pct` are
sourced from BLS and the primary paper, `Transition_Rate` and
`Attrition_Rate` are Jennifer's confirmed judgment calls, and
`Base_MidLevel_Stock` is now the real weighted mid-level headcount for
Graphic Designers from the IPUMS extract (see §2 below for sourcing on
each). Formula correctness was verified against an independent plain-Python
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
| `Base_MidLevel_Stock` | 98,685 | IPUMS CPS Basic Monthly, OCC 2634 (Graphic Designers), employed (EMPSTAT 10/12), age 26-35, 2022. Weighted headcount averaged across the 12 monthly samples (range 77,280-117,161 month to month; unweighted N=350 across the year, roughly 23-68 respondents/month). Age band chosen as the complement to the 22-25 early-career band already used for H1, since the model's `MidSupply` recursion has no separate exit tier for "senior" — attrition is the only way out of the stock, so "mid-level" is simply everyone employed in the occupation who isn't early-career. |

All five Inputs values are now real or confirmed.

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

**Pass 3 (re-run with all five values real/confirmed — `Base_MidLevel_Stock`
now 98,685, replacing the 500 placeholder; the other four unchanged from
Pass 2):**

| | N=3 | N=5 |
|---|---|---|
| Onset year (Depressed) | 2026 | 2028 |
| Max gap (Depressed) | ~17,688 | ~14,836 |
| Baseline stock at 2035 | ~198,603 | ~198,603 |
| Onset year (Recovery) | 2026 | 2028 |
| Max gap (Recovery) | ~9,972 | ~9,972 |

All three passes confirm the same onset-timing pattern: N=3's onset (2026)
is `Base_Year+1+N` = 2023+3; N=5's onset (2028) is 2023+5 — shorter
training lag means the shortage shows up sooner, not later, under otherwise
identical hiring assumptions.

**Correction to what Pass 2 claimed about the gap magnitudes.** Pass 2's
writeup said the gap numbers were "not meaningful" because they were
computed against the placeholder stock. Comparing Pass 2 and Pass 3
directly shows that claim was wrong: every `Max gap` figure is identical
between the two passes, even though `Base_MidLevel_Stock` changed from 500
to 98,685. This isn't a coincidence — it falls out of the model's own
structure. `Gap[t,s,N] = MidSupply[t,Baseline,N] - MidSupply[t,s,N]`, and
all three scenarios share the same seed value in `Base_Year` and the same
attrition decay each year; that shared term cancels exactly out of the
subtraction, leaving the gap a function of the hiring-flow differences
alone. `Base_MidLevel_Stock` only ever affected the *absolute* baseline
level (148,201 vs. 198,603 at `End_Year`) — never the gap, which is what
H2 actually asks about. So Pass 2's gap numbers were already H2's answer;
they just hadn't been labeled that way. Pass 3 confirms it rather than
changing it.

## 6. Cross-occupation extension (Figure 2)

Figure 2 runs the same recursion (N=3, the primary case only) separately
for three occupations, each with its own real inputs:

| Occupation | `Baseline_Annual_Hires` | `Depressed_Hiring_Pct` | `Base_MidLevel_Stock` |
|---|---|---|---|
| Graphic Designers | 20,000 (BLS OOH) | 0.855 (Quintile 4, −14.5%, Table 1 Panel A) | 98,685 (IPUMS, age 26-35) |
| Software Developers | 134,600 (BLS OOH, **2018–28 cycle** — older vintage than the other two occupations' current-cycle figures; flagged, not reconciled) | 0.82 (Quintile 5, −18%, Table 1 Panel A, primary 2018-balanced sample — same specification as Graphic Designers' figure) | 758,181 (IPUMS, age 26-35, N=2,630) |
| Home Health Aides | 765,800 (BLS OOH, current cycle) | 1.0 — **inferred, not read off a table cell.** Home Health Aides is Quintile 1 (lowest exposure, confirmed via Table A.2). The paper's text discusses "the least-exposed quintile" only as a comparison group and never gives it its own coefficient, consistent with Quintile 1 being the omitted regression reference category (coefficient = 0 by construction). This is a reasonable inference, not a confirmed figure — worth checking against an explicit statement in the paper if one is ever found. | 108,974 (IPUMS, age 26-35, N=329) |

`Transition_Rate` (0.80) and `Attrition_Rate` (0.05) are unchanged across
all three — Jennifer's judgment calls, not occupation-specific, same as
the single-occupation model.

Quintile assignments for Software Developers and Home Health Aides came
from the paper's Table A.5/A.6 appendix (top-50-occupations-by-ADP-
employment lists, one per quintile) — Interface Designers was checked
against all five quintile tables and doesn't appear in any of them, so
no `Depressed_Hiring_Pct` could be sourced for it and it's excluded from
Figure 2.

Because Software Developers' hiring volume is roughly 6-7x Graphic
Designers', the gap is reported as a percentage of each occupation's own
baseline-scenario stock rather than raw headcount — plotting absolute
counts together would make Graphic Designers' line nearly invisible next
to Software Developers'. At N=3: Graphic Designers peaks at ~8.9% (2035,
Depressed), Software Developers at ~10.7%, and Home Health Aides stays
at 0% throughout (a direct consequence of the `Depressed_Hiring_Pct` = 1.0
assumption above, not an independent finding).
