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

This paper examines whether the adoption of generative AI is weakening the early-career hiring pipeline in occupations where AI can perform or augment entry-level work, and what that shift could mean for the future supply of experienced workers. The analysis focuses on four occupations—Graphic Designers, Web & Digital Interface Designers, Software Developers, and Home Health Aides—and compares changes in early-career employment with experienced-worker employment to assess whether observed declines are concentrated among workers at the beginning of their careers.

The paper has three objectives. First, it tests whether early-career employment has declined more sharply for Graphic Designers than for Web & Digital Interface Designers, while using the Home Health Aide control to examine whether the pattern is concentrated among younger workers rather than reflecting a broader occupational downturn. Second, it assesses whether a sustained reduction in junior hiring could create a downstream gap in the experienced-worker pipeline, using an explicit cohort-based projection and sensitivity analysis around the assumed time required for junior workers to progress to mid-level roles. Third, it evaluates the implications for firms and policymakers and proposes a collective response that preserves early-career pathways and shared investment in the future talent pipeline.

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
| IPUMS CPS, OCC 3601 (Home Health Aides) | Employment by age — same-source control for H1 | Sourced; code confirmed against IPUMS CPS codebook (`cps.ipums.org/cps/codes/occ_2020_codes.shtml`) |
| Brynjolfsson, Chandar & Chen (2025, revised Aug 2026), "Canaries in the Coal Mine?" | Primary-paper citation for the wage-vs-employment mechanism (Fact 6, pp. 21-22); AI-exposure coefficient for `Depressed_Hiring_Pct` (Table 1, Panel A) | Sourced. Also the source behind the Stanford/ADP Canaries Dashboard — same underlying research, not an independent second source, so no separate dashboard pull planned |
| BLS Occupational Outlook Handbook (OOH), Graphic Designers | ~20,000 average annual openings → `Baseline_Annual_Hires` in the pipeline model | Sourced |
| Anthropic Economic Index, Job Explorer (June 2026 "Cadences") | Task-level automation/augmentation grid for Graphic Designers and Interface Designers | Sourced |
| | | |

## 3. Models & Figures to Build

**Models**

| Model | What it does | Status |
|---|---|---|
| Early-career vs. experienced employment index | CPS-derived weighted employment index for Graphic Designers, Interface Designers, and Home Health Aides (all three pulled from IPUMS); Software Developers' series comes from the primary paper rather than a fresh CPS pull | Built — see `capabilities/economic-research/` method (H1) |
| H1 hypothesis test | Weighted decline ratio per occupation, count-based SE, 95% CI on the GD-vs-ID gap, with the HHA control checked separately for a general-downturn confound | Run — real result in the brief's `[CELL-SIZE NOTE]` |
| Pipeline-projection cohort model (H2) | Cohort model tracking junior hires through a training lag (N=3 primary, N=5 sensitivity) into mid-level stock, under Depressed and Recovery hiring scenarios | Built — `capabilities/economic-research/model.xlsx` (method: `pipeline-model-spec.md`) |

**Figures**

| Figure | Content | Placement |
|---|---|---|
| Figure 1 (required) | Early-career employment index, all four occupations, over time, one chart | Main body |
| Figure 2 (candidate) | Pipeline projection — the gap between junior-hiring recovery and when mid-level talent is actually needed | Appendix if main-body space is tight; the H2 finding itself still belongs in the recommendation either way |

Both figures are still unbuilt as rendered chart images — the underlying data/model exists, but nothing's been drawn yet.

## 4. Success Criteria

> Decided before the result is known — what a finished analysis has to
> show to count as support, as non-support, or as inconclusive. This is
> the section that keeps a null result from quietly becoming "the data
> were too noisy to tell."

## 5. Validation Rules

> Internal consistency checks to run before trusting a number — e.g. do
> an independent calculation and a model's formula agree; does a
> weighted total move the way an unweighted count says it should.

## 6. Output Format

Fixed by the assignment (adamwstauffer.github.io/ai-lms/research-paper.html#rubric) —
not a judgment call, copied here for reference:

- Four pages maximum (not counting title page, graphs, bibliography, appendix)
- Typed, double-spaced, 12-point Times New Roman, one-inch margins
- Title page: name, date, course title, assignment title — the only page with identifying information
- At least one graph, chart, or diagram
- Citations in APA, MLA, or Chicago, with a separate bibliography page
- The repository URL never appears anywhere in the paper (double-anonymous peer review)

## References

> Sources used to write this spec itself (not the paper's bibliography).
