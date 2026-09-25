---
engagement: economic-research
stage: 1 · Ask
file: docs/briefs/research-brief.md
date: 2026-09-25
status: draft v2 (roles revised to a within-design split + benchmark + control; thesis sharpened to market self-correction; AI-assisted scaffold, rewrite in my own words before push)
scope: United States
decision-owner: Employers of design and engineering talent, and the professional bodies that shape the pipeline
question: >
  AI is removing the entry-level tasks junior designers and engineers used to
  learn on. Will the market correct the resulting training shortfall on its own
  in time, and if not, what is the most targeted fix?
---

# Research Brief — The Missing Rung: AI and the Entry-Level Tech Pipeline

## Thesis (working)

AI is not eliminating technology careers so much as disrupting the traditional entry point into them. While rising wages for experienced technology professionals may eventually indicate a shortage of skilled talent, the long development cycle for building that expertise means the market may recognize the shortage only after the pipeline of early-career talent has already weakened. To sustain the long-term talent pipeline, firms need to develop mechanisms for collectively investing in the training and development of early-career professionals.

## The problem

**What it is.** Generative AI now does much of the routine work junior designers and engineers used to learn on: production assets and layouts, boilerplate code, and first-draft wireframes. Firms can get that output without hiring a junior. Payroll evidence suggests this is already happening. In Stanford Digital Economy Lab research using ADP data, employment of 22–25-year-olds in the most AI-exposed occupations sits 19% below where it would be had it kept pace with less-exposed peers, while experienced workers show no comparable gap (Brynjolfsson, Chandar & Chen, revised August 2026). The adjustment runs mainly through hiring rather than wages, and it is concentrated where AI *automates* tasks rather than *augments* them.

**Who it affects.**
- Early-career designers and engineers: they can't get the first job.
- Employers: next year's cost savings become a shortage of mid-level talent in three to five years.
- Senior practitioners: short-run winners, whose scarcity raises their value.
- The profession: thinner succession and less diversity of background in the pipeline.

**Why now, not in general.** Every technology shifts tasks. What is new is (1) the speed of adoption since late 2022, and (2) that the tasks being automated are specifically *the training tasks*. Entry-level work used to pay for itself while teaching the craft. If that arrangement breaks, the effect appears years later as a missing mid-level cohort.

## The comparison — designed to test the mechanism

| Role (federal occupation) | Junior work | AI's main effect on junior work | Expected early-career effect |
|---|---|---|---|
| **Graphic designers** | Production assets, layouts, variations | Automates | Large decline |
| **Software developers** (benchmark) | Boilerplate code, tests, simple features | Automates | Large decline; best-documented case |
| **Web & digital interface designers** | Flows, interaction design, user needs | Mostly augments | Smaller decline |
| **Low-exposure control** `[CHOOSE from dashboard: e.g., nursing assistants, electricians]` | Hands-on work AI barely touches | Minimal | Roughly flat |

**Why this design.**
- **The within-design split is the heart of it.** Graphic designers and interface designers are in the same field, but their junior tasks differ in how automatable they are. If their early-career paths diverge, the mechanism is automatability, not "design is being replaced." This is where my professional judgment adds the most.
- **Software developers anchor the pattern** in the best-documented case.
- **The control rules out the boring explanation**, that the job market is simply bad for young people right now. The control is a methodological check, not the thesis.

`[CHECK FIRST: confirm all four occupations have early-career series on the Stanford Canaries Dashboard.]`

## Economic concepts this touches

- **Externalities (Session 4).** Training a junior creates value the training firm doesn't fully capture, because trained workers can leave. That's an unpriced positive externality, so the market underproduces training. This is the core mechanism.
- **Tragedy of the commons (Session 4).** The industry's pool of experienced talent is a shared resource that each firm draws from without replenishing.
- **Short-run vs. long-run elasticity (Session 2).** Firms cut junior hiring immediately, but mid-level supply can't respond for years. When quantity can't adjust, price does.
- **Specificity rule (Sessions 4 and 7).** The root cause is the lost training subsidy, not AI itself, so the fix should target training, not slow adoption.
- **Who gained and who paid (Session 2).** Firms and seniors gain; juniors lose access to the ladder.
- **Macro link (Session 7; Ch. 14–15).** Human capital formation and long-run productivity growth; headline employment that hides a cohort-specific decline.

## The contested question — will the market fix it?

This is where the analysis lives.

- **The case for self-correction:** as mid-level talent grows scarce, senior wages rise, and that price signal pushes firms back into training. Or AI shortens the time to competence, so a smaller pipeline is efficient.
- **The case against:** the price signal arrives years after the hiring decision, and no single firm can capture the return on training, so each waits for others. The Stanford finding that adjustment has come through employment rather than wages suggests the price signal isn't showing up yet.
- **My position to test:** self-correction fails on *timing*, not on direction.

## What I am assuming

| Assumption | Source status |
|---|---|
| Early-career employment in AI-exposed occupations has declined relative to experienced workers | Brynjolfsson, Chandar & Chen (Aug 2026). Descriptive, not causal, per the authors |
| Adjustment so far has come through employment, not wages | Same source. Verify the exact finding in the primary paper |
| Junior-to-mid-level progression takes about 3–5 years | `[TBD: source, or state as my professional judgment]` |
| Graphic designers' junior tasks are more automatable than interface designers' | My professional judgment. `[CHECK: occupation- or task-level automation vs. augmentation data]` |
| The decline is driven by AI rather than interest rates or the post-2022 tech correction | The control group plus the Stanford controls. **Biggest vulnerability; address it directly** |

## The analysis I plan to run

1. **Descriptive:** an early-career vs. experienced employment index for all four occupations, late 2022 to present.
2. **Mechanism test:** does the size of the early-career gap follow the automatability ranking?
3. **Price-signal check:** are wages for experienced workers in these occupations rising yet? `[SOURCE: BLS wage data or the Stanford compensation findings]`
4. **Pipeline projection (Excel):** a simple cohort model. Juniors hired each year become mid-level after `[N]` years, with an assumed attrition rate. Two named scenarios:
   - **Depressed:** junior hiring stays at the current reduced level through 2030.
   - **Recovery:** junior hiring returns to its pre-2022 trend by 2028.
   Output: the projected mid-level gap, and the year a wage signal would plausibly appear versus the year trained people would be needed.

**Figure 1 (required):** the early-career employment index for all four occupations over time, on one chart. Graphic and interface designers diverging, with the control flat, is the evidence.
**Figure 2 (if space allows):** the pipeline projection, showing the gap between when the shortage signals and when the talent is needed.

## Hypotheses

**H1 (mechanism):** since late 2022, the relative early-career employment decline is larger for graphic designers than for web and digital interface designers, by at least `[X]` percentage points. Software developers fall closer to graphic designers, and the control shows no meaningful decline.

**H2 (timing):** under the depressed scenario, a mid-level shortage emerges by `[YEAR]`, while the time needed to train replacements exceeds the time between the wage signal appearing and the talent being needed.

> Commit `[X]` and `[YEAR]` before pulling data or building the model.

## How I would know I was wrong

- **Graphic and interface designers decline by about the same amount.** Automatability isn't the driver; the story becomes general junior-hiring weakness, and the recommendation changes.
- **The control declines too.** The pattern is a youth labor-market story, not an AI story. The paper would have to say so.
- **Experienced wages are already rising sharply.** The price signal is working faster than I assume, and self-correction may be viable. My recommendation would shrink to "monitor."
- **The early-career decline predates late 2022.** The AI explanation weakens; check the pre-trend before building on the data.

## Recommendation directions (to be decided by the analysis, not before it)

- **Shared training pools or apprenticeship consortia:** firms co-fund training so no single firm bears the cost others capture. This is the most direct fix for the externality.
- **Redesigned junior roles:** juniors direct, review, and correct AI output instead of producing what AI now does.
- **Policy:** extend apprenticeship funding or entry-level hiring credits to design and engineering roles.
- **Professional associations:** structured pipelines from education to a first job.

**Obvious objection to defend against:** "The market will sort it out: senior wages will rise and firms will start training again." My answer should come from the timing analysis. The signal is real, but it arrives after the window to act has closed.

## Data to gather

- Stanford Digital Economy Lab, Canaries Dashboard: early-career and experienced series for the four occupations
- Brynjolfsson, Chandar & Chen (2025, revised Aug 2026), "Canaries in the Coal Mine?": cite the primary paper
- BLS Occupational Employment and Wage Statistics: employment and wages for graphic designers, web & digital interface designers, software developers, and the control
- Task-level evidence on AI automation vs. augmentation by occupation
- A source for typical junior-to-mid-level progression time

## Guardrails for the paper

- No employer or organization names that identify me in the body.
- Every number from a public, citable source. Label my professional judgment as judgment.
- No repository URL anywhere in the paper.
- Four pages maximum, excluding title page, figures, bibliography, and appendix. Each occupation gets only the words its contribution to the comparison needs.
