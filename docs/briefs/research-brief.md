---
engagement: economic-research
stage: 1 · Ask
file: docs/briefs/research-brief.md
date: 2026-10-06
status: draft v3
scope: United States
decision-owner: Employers and industry leaders responsible for developing early-career talent; professional associations and industry groups as potential coordinators
question: >
  AI is removing the entry-level tasks junior designers and engineers used to
  learn on. Will the market correct the resulting training shortfall on its own
  in time, and if not, what is the most targeted fix?
---

# Research Brief — The Missing Rung: AI and the Entry-Level Tech Pipeline

## Thesis (working)

AI is not eliminating technology careers so much as disrupting the traditional entry point into them. As firms reduce junior hiring, the resulting gap in experienced talent may not become visible until those smaller cohorts would have progressed into mid-level roles, at which point employers may respond by increasing entry-level hiring and training. Because those new hires cannot immediately replace the experienced workers the market lacks, however, hiring can recover only after the pipeline has already weakened. To sustain the long-term talent pipeline, firms need mechanisms for collectively investing in the training and development of early-career professionals.

## The problem

**What it is.** Generative AI now does much of the routine work junior designers and engineers used to learn on: production assets and layouts, boilerplate code, and first-draft wireframes. Firms can get that output without hiring a junior. Payroll evidence suggests this is already happening. In Stanford Digital Economy Lab research using ADP data, employment of 22–25-year-olds in the most AI-exposed occupations sits 19% below where it would be had it kept pace with less-exposed peers, while experienced workers show no comparable gap (Brynjolfsson, Chandar & Chen, revised August 2026). The adjustment runs mainly through hiring rather than wages, and it is concentrated where AI *automates* tasks rather than *augments* them.

**Who it affects.**
- Early-career designers and engineers: they can't get the first job.
- Employers: next year's cost savings become a shortage of mid-level talent in about three years.
- Senior practitioners: short-run winners, whose scarcity raises their value.
- The profession: thinner succession and less diversity of background in the pipeline.

**Why now, not in general.** Every technology shifts tasks. What is new is (1) the speed of adoption since late 2022, and (2) that the tasks being automated are specifically *the training tasks*. Entry-level work used to pay for itself while teaching the craft. If that arrangement breaks, the effect appears years later as a missing mid-level cohort.

## The comparison — designed to test the mechanism

| Role (federal occupation) | Junior work | AI's main effect on junior work | Expected early-career effect |
|---|---|---|---|
| **Graphic designers** | Production assets, layouts, variations | Automates | Large decline |
| **Software developers** (benchmark) | Boilerplate code, tests, simple features | Automates | Large decline; best-documented case |
| **Web & digital interface designers** | Flows, interaction design, user needs | Mostly augments | Smaller decline |
| **Low-exposure control: Home Health Aide** | Hands-on work AI barely touches | Minimal | Roughly flat |

**Why this design.**
- **Software developers anchor the pattern** in the best-documented case, providing a useful benchmark for what an AI-disrupted early-career occupation can look like.
- **The GD–interface-designer split is where my professional judgment adds the most.** Both occupations come from CPS, allowing me to test whether differences in AI exposure correspond to different early-career employment patterns within the same data source.
- **The control provides a benchmark for broader labor-market conditions**, helping distinguish an AI-related effect from a general deterioration in employment for young workers. The Stanford Canaries Dashboard's public interface confirms early-career series for Software Developers and Home Health Aides but not for Graphic Designers or Web & Digital Interface Designers, so those two occupations are measured using CPS/IPUMS microdata instead (see "Data to gather"). One limitation is that ADP captures payroll records at ADP-client firms, while CPS captures self-reported employment across employer types. The age bands match exactly (22–25), so the issue is coverage, not age. This affects the control comparison and Figure 1, but not the within-CPS mechanism test. I retain the ADP control because its externally documented evidence provides a valuable benchmark; I therefore treat the cross-source comparison as contextual evidence, not a like-for-like control.
- **The early-career-versus-experienced employment index already planned for all four occupations serves a second purpose** for the two CPS-sourced occupations: it tests whether the decline is concentrated among younger workers rather than reflecting a broad occupation-wide downturn. A general downturn should affect experienced workers as well; an AI-specific effect should be more concentrated among workers at the entry level, where AI can substitute for the tasks through which workers traditionally build experience.

## Economic concepts this touches

- **Externalities (Session 4).** Training a junior creates value the training firm doesn't fully capture, because trained workers can leave. That's an unpriced positive externality, so the market underproduces training. This is the core mechanism — it explains why firms underinvest, and it's why firms are this brief's decision-owner.
- **Short-run vs. long-run elasticity (Session 2).** Firms can cut junior hiring immediately, but mid-level supply cannot respond as quickly because developing experienced talent takes years. Hiring quantity eventually adjusts — but only after that development lag, creating a pipeline gap in the interim. H2 tests this mechanism quantitatively.
- **Specificity rule (Sessions 4 and 7).** The root cause is the lost training subsidy, not AI itself, so the fix should target training, not slow adoption. This is the bridge from evidence to recommendation.

## The contested question — will the market fix it?

This is where the analysis lives.

- **The case for self-correction:** as the smaller cohorts of junior workers progress through the pipeline, firms may eventually face a shortage of mid-level talent and respond by increasing junior hiring and training. Or AI may shorten the time to competence, making a smaller entry-level pipeline efficient.
- **The case against:** the hiring response may come only after the shortage becomes visible. Firms can reduce junior hiring immediately when AI makes entry-level tasks less valuable, but increasing junior hiring in response to a future mid-level shortage does not immediately replenish the experienced talent pool. The Stanford finding that recent adjustment has occurred through employment rather than wages is consistent with employment being the relevant adjustment channel, but it does not establish that the recovery phase has begun. The key question is whether junior hiring begins to recover before the resulting cohort gap becomes a shortage of experienced workers.
- **My position to test:** self-correction fails on *timing*, not on direction.

## What I am assuming

| Assumption | Source status |
|---|---|
| Early-career employment in AI-exposed occupations has declined relative to experienced workers | Brynjolfsson, Chandar & Chen (Aug 2026). Descriptive, not causal, per the authors |
| Adjustment so far has come through employment, not wages | Same source. Verify the exact finding in the primary paper |
| Junior-to-mid-level progression takes about three years | My professional judgment about the development period relevant to this pipeline — a modeling assumption, not an externally established benchmark. Tested at five years as a sensitivity case. |
| Graphic designers' junior tasks are more automatable than interface designers' | Anthropic Economic Index Job Explorer classifies both occupations separately with their own automation/augmentation measures, confirmed by direct check. `[TBD: pull the actual percentages and confirm they support this ranking, not just that the data exists]` |
| The decline is driven by AI rather than interest rates or the post-2022 tech correction | The control group plus the Stanford controls. **Biggest vulnerability; address it directly** |

## The analysis I plan to run

1. **Descriptive:** an early-career vs. experienced employment index for all four occupations, late 2022 to present.
2. **Mechanism and recovery test:** does the size of the early-career employment gap follow the automatability ranking, and does junior employment show any evidence of recovery after its initial decline? Use the same employment series to distinguish the initial hiring contraction from a subsequent corrective response.
3. **Pipeline projection (Excel):** a simple cohort model. Juniors hired each year become mid-level after three years (primary case), with an assumed attrition rate; a five-year transition is tested as a sensitivity case. Two named scenarios:
   - **Depressed:** junior hiring stays at the current reduced level through 2030.
   - **Recovery:** junior hiring returns to its pre-2022 trend by 2028.
   Output: the projected mid-level gap, and the lag between the recovery in junior hiring and the availability of experienced talent.

**Figure 1 (required, main body):** the early-career employment index for all four occupations over time, on one chart. Graphic and interface designers diverging, with the control flat, is the evidence that this pattern is happening.
**Figure 2 (appendix candidate):** the pipeline projection, showing the gap between when junior hiring begins to recover and when the talent is needed. Figure 1 establishes the pattern; Figure 2 translates it into what the lag could mean for the future. If main-body space is tight, the detailed chart moves to the appendix — but the H2/pipeline-model finding it's based on still belongs in the recommendation, not just the appendix.

## Hypotheses

**H1 (mechanism):** since late 2022, **graphic designers decline more than Web & Digital Interface Designers.** Because these estimates come from relatively small CPS/IPUMS cells, the comparison will be evaluated using the 95% confidence interval for the estimated difference; the result supports the hypothesis if the interval excludes zero in the predicted direction. Software developers fall closer to graphic designers, and the control shows no meaningful decline.

`[CELL-SIZE NOTE: pooling the full late-2022-to-present window into two ~8-quarter blocks (baseline, current) gives roughly 128 CPS respondents per occupation per block. At that size, each block's estimate carries a standard error of roughly 4.4 percentage points; the between-occupation comparison's standard error is roughly 8.8 points, and the resulting 95% confidence interval on the gap is roughly ±17 points wide. These are order-of-magnitude estimates — a simple-random-sample approximation, not a design-based CPS calculation — and should be replaced with the actual figures once the IPUMS extract is built.]`

**H2 (timing):** under the depressed scenario, a mid-level shortage emerges by 2030, and the subsequent recovery in junior hiring will not arrive early enough to build the talent pipeline before that talent is needed, given the assumed training time. `[PROVISIONAL: this claim predates the pipeline model build. The primary training-time assumption just moved from five years to three, which changes the cohort math — re-verify against the model once built, under both the three-year primary case and the five-year sensitivity case.]`


## How I would know I was wrong

- **Graphic and interface designers decline by about the same amount.** Automatability isn't the driver; the story becomes general junior-hiring weakness, and the recommendation changes.
- **The control declines too.** The pattern is a youth labor-market story, not an AI story. The paper would have to say so.
- **Junior hiring is already recovering sharply.** The hiring signal is working faster than I assume, and self-correction may be viable. My recommendation would shrink to "monitor."
- **The early-career decline predates late 2022.** The AI explanation weakens; check the pre-trend before building on the data.

## Recommendation directions (to be decided by the analysis, not before it)

The disruption of entry-level roles is not only an individual firm's problem but a
collective training problem: firms have less incentive to invest when they may not
capture the full return on that investment. The most direct response is collective
action among firms.

- **Shared training pools or apprenticeship consortia:** firms co-fund training so no
  single firm bears the cost others capture — the most direct fix for the externality,
  with professional associations and industry groups serving as potential coordinators.
- **Other directions, in support rather than in place of firm-level action:** redesigned
  junior roles (juniors direct, review, and correct AI output instead of producing what
  AI now does); policy support such as apprenticeship funding or entry-level hiring
  credits. Policymakers have a supporting role, but they are not the primary actor in
  this argument.

**Obvious objection to defend against:** "The market will sort it out: junior hiring will recover and firms will rebuild the pipeline." My answer should come from the timing analysis. Junior hiring may eventually recover, but the recovery will arrive after the window to build the pipeline has closed.

## Data to gather

- Stanford Digital Economy Lab, Canaries Dashboard: early-career and experienced series for Software Developers and Home Health Aides
- CPS/IPUMS: employment by age for Graphic Designers (OCC 2634) and Web & Digital Interface Designers (OCC 1032); assess quarterly pooling after checking actual cell sizes
- Brynjolfsson, Chandar & Chen (2025, revised Aug 2026), "Canaries in the Coal Mine?": cite the primary paper
- BLS Occupational Employment and Wage Statistics: employment and wages for graphic designers, web & digital interface designers, software developers, and the control
- Anthropic Economic Index, Job Explorer: occupation-specific automation vs. augmentation measures for Graphic Designers and Web & Digital Interface Designers
- A source for typical junior-to-mid-level progression time

## Guardrails for the paper

- No employer or organization names that identify me in the body.
- Every number from a public, citable source. Label my professional judgment as judgment.
- No repository URL anywhere in the paper.
- Four pages maximum, excluding title page, figures, bibliography, and appendix. Each occupation gets only the words its contribution to the comparison needs.
