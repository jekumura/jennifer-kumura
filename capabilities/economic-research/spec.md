---
type: spec
capability: economic-research
engagement: economic-research
date: 2026-10-09
version: 0.1
status: draft
---

# Economic Research — spec

**Source brief:** [`docs/briefs/2026-10-09-research-brief.md`](../../docs/briefs/2026-10-09-research-brief.md)

> Per the assignment page (adamwstauffer.github.io/ai-lms/research-paper.html,
> Step 2 · Specify): "Data sources, the model and figures you intend to
> build, and the success criteria a finished paper has to meet." Writing
> this is yours, human-first — same as the brief. A spec written before
> the research starts is what stops the paper from becoming whatever the
> sources happened to be about.

## 1. Scope & Objective

This paper examines whether the adoption of generative AI is weakening the early-career hiring pipeline in occupations where AI can perform or augment entry-level work, and what that shift could mean for the future supply of experienced workers. The analysis focuses on four occupations—Graphic Designers, Web & Digital Interface Designers, Software Developers, and Construction Laborers—and compares changes in early-career employment with experienced-worker employment to assess whether observed declines are concentrated among workers at the beginning of their careers.

The paper has three objectives. First, it tests whether early-career employment has declined more sharply for Graphic Designers than for Web & Digital Interface Designers, while using the Construction Laborers control to examine whether the pattern is concentrated among younger workers rather than reflecting a broader occupational downturn. Second, it assesses whether a sustained reduction in junior hiring could create a downstream gap in the experienced-worker pipeline, using an explicit cohort-based projection and sensitivity analysis around the assumed time required for junior workers to progress to mid-level roles. Third, it evaluates the implications for firms and policymakers and proposes a collective response that preserves early-career pathways and shared investment in the future talent pipeline.

The analysis is deliberately narrower than a claim that AI has caused overall employment declines or will eliminate entire occupations. Evidence of changing employment patterns is treated separately from AI-exposure measures, and the cohort projection is presented as a scenario rather than a forecast. The objective is therefore to determine whether the emerging evidence is consistent with an AI-driven disruption to the traditional entry-level-to-experienced talent pipeline and, if so, identify a practical institutional response.

## 2. Data Sources

> Every data source the paper will draw on, named specifically enough
> that it could be looked up by someone else. For each: what it is, what
> it's used for, and its current status (sourced / in progress / still
> needed).

| Source | Used for | Status |
|---|---|---|
| IPUMS CPS, OCC 2634 (Graphic Designers) | Employment by age — early-career (22-25) index for H1; mid-level (26-35) headcount for H2's `Base_MidLevel_Stock` | Sourced |
| IPUMS CPS, OCC 1032 (Web & Digital Interface Designers) | Employment by age — early-career (22-25) index for H1 | Sourced |
| IPUMS CPS, OCC 6260 (Construction Laborers) | Employment by age — same-source control for H1; mid-level (26-35) headcount for Figure 2's cross-occupation extension | Sourced. Replaces OCC 3601 (Home Health Aides), which was sourced and code-confirmed first but showed a 22.3% decline rather than the flat pattern expected of a control. |
| Brynjolfsson, Chandar & Chen (2025, revised Aug 2026), "Canaries in the Coal Mine?" | Primary-paper citation for the wage-vs-employment mechanism (Fact 6, pp. 21-22); AI-exposure coefficient for `Depressed_Hiring_Pct` (Table 1, Panel A) | Sourced. Also the source behind the Stanford/ADP Canaries Dashboard — same underlying research, not an independent second source, so no separate dashboard pull planned |
| BLS Occupational Outlook Handbook (OOH), Graphic Designers | ~20,000 average annual openings → `Baseline_Annual_Hires` in the pipeline model | Sourced |
| Anthropic Economic Index, Job Explorer (June 2026 "Cadences") | Task-level automation/augmentation grid for Graphic Designers and Interface Designers | Sourced |
| | | |

## 3. Models & Figures to Build

**Models**

| Model | What it does | Status |
|---|---|---|
| Early-career vs. experienced employment index | CPS-derived weighted employment index for Graphic Designers, Interface Designers, and Construction Laborers (all three pulled from IPUMS); Software Developers' series also pulled from the same IPUMS extract | Built — see `capabilities/economic-research/` method (H1) |
| H1 hypothesis test | Weighted decline ratio per occupation, count-based SE, 95% CI on the GD-vs-ID gap, with the Construction Laborers control checked separately for a general-downturn confound | Run — real result in the brief's `[CELL-SIZE NOTE]` |
| Pipeline-projection cohort model (H2) | Cohort model tracking junior hires through a training lag (N=3 primary, N=5 sensitivity) into mid-level stock, under Depressed and Recovery hiring scenarios | Built — `capabilities/economic-research/model.xlsx` (method: `pipeline-model-spec.md`) |

**Figures**

| Figure | Content | Placement |
|---|---|---|
| Figure 1 (required) | Early-career employment index, all four occupations, over time, one chart | Main body |
| Figure 2 (candidate) | Pipeline projection — the gap between junior-hiring recovery and when mid-level talent is actually needed | Appendix if main-body space is tight; the H2 finding itself still belongs in the recommendation either way |

Both figures are still unbuilt as rendered chart images — the underlying data/model exists, but nothing's been drawn yet.

## 4. Success Criteria

**H1 (mechanism — Graphic Designers vs. Interface Designers)**

- *Supported* if the 95% CI on the GD−ID decline gap excludes zero, with GD's decline the larger one (gap positive).
- *Contrary* if the CI excludes zero in the opposite direction (ID declined more).
- *Inconclusive* if the CI includes zero — the cells are too small to distinguish.
- **Actual result:** inconclusive, and the point estimate ran opposite the predicted direction (gap −13.0pp, 95% CI −38.6 to +12.6). See the brief's `[CELL-SIZE NOTE]`.

*Falsification conditions carried over from the brief:* GD and ID decline by about the same amount (automatability isn't the driver); experienced workers decline alongside early-career workers in the same occupations, or the Construction Laborers control shows a comparable decline (pattern isn't entry-level-specific — not triggered: the control declined only 1.83%, consistent with a flat control rather than a comparable decline); the early-career decline predates late 2022 (AI explanation weakens).

**H2 (timing — pipeline shortage)**

- *Supported* if, under the Depressed scenario, the shortage (Gap > 0) emerges before 2030 at both N=3 and N=5, and the Recovery scenario's gap does not fully close by `End_Year`.
- *Not supported* if the shortage emerges after 2030, or Recovery fully closes the gap well before the talent is needed.
- **Actual result:** supported — onset 2026 (N=3) / 2028 (N=5), both before 2030; Recovery's gap persists through 2035.

*Falsification condition:* junior hiring is already recovering sharply — self-correction may be viable, recommendation shrinks to "monitor."

## 5. Validation Rules

**For the CPS/IPUMS employment index (H1):**

- Unweighted respondent counts must match before and after any filter change for the right reason — when the EMPSTAT employed-only filter was added, counts dropped by exactly the number of non-employed rows identified separately (confirms the filter logic, not a silent data change).
- A weighted total (`sum(WTFINL)`) should never return `NA` for an employed-only block; if it does, that's a missing-weight bug, not a real zero — resolve the cause before adding `na.rm = TRUE`, don't just suppress it.
- Quarterly cell counts should be continuous across the pooled window with no isolated gaps or spikes — a break usually means a sample-vintage mixup (e.g. an ASEC supplement accidentally pulled alongside Basic Monthly samples), not a real data discontinuity.
- Any occupation code must be checked against the primary IPUMS codebook for the scheme vintage in use, not a secondary source (forum post, aggregator) alone.

**For the pipeline cohort model (H2):**

- The Excel formulas must match an independent plain-Python simulation of the same recursion before the result is trusted — this is how a real error gets caught (see `pipeline-model-spec.md` §5, where this check also caught and corrected an earlier wrong claim about which numbers `Base_MidLevel_Stock` actually affects).
- Re-running the simulation with a changed input should move only the outputs that input can structurally affect — e.g. changing `Base_MidLevel_Stock` should move the absolute baseline level but not the Gap, since all three scenarios share the same seed and decay.

## 6. Output Format

Fixed by the assignment (adamwstauffer.github.io/ai-lms/research-paper.html#rubric) —
not a judgment call, copied here for reference:

- Four pages maximum (not counting title page, graphs, bibliography, appendix)
- Typed, double-spaced, 12-point Times New Roman, one-inch margins
- Title page: name, date, course title, assignment title — the only page with identifying information
- At least one graph, chart, or diagram
- Citations in **APA** (chosen format), with a separate bibliography page
- The repository URL never appears anywhere in the paper (double-anonymous peer review)

## References

Sources used to write this spec itself (not the paper's bibliography):

- `docs/briefs/2026-10-09-research-brief.md` — the brief this spec is built from
- Stauffer, A. (n.d.). *Research paper assignment*. AI + LMS course site. https://adamwstauffer.github.io/ai-lms/research-paper.html — assignment requirements, workflow stages, and Output Format constraints
- `docs/templates/spec-template.md` — the template this spec adapts (sections 6-10 and the ratio-specific sections dropped as not applicable)
- `capabilities/economic-research/pipeline-model-spec.md` — source for the H2 model/figure details and the independent-verification checks in §5
- IPUMS CPS occupation codes, 2020+ scheme. https://cps.ipums.org/cps/codes/occ_2020_codes.shtml — OCC 6260 code confirmation, carried into §2
