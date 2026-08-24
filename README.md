# jennifer-kumura

Personal working repo for strategy and transformation engagements — a place to run each engagement through a repeatable cycle (brief → build → analyze → decide), build up reusable analytical capabilities along the way, and keep a record of what AI helped with. See [AGENTS.md](./AGENTS.md) for the full AI working conventions.

## Repo structure

| Folder | What lives there |
|---|---|
| `docs/briefs/` | Written **before** work starts: an engagement's scope and hypothesis. |
| `docs/decisions/` | Written **after** work finishes: the recommendation, addressed to a specific audience. |
| `capabilities/<name>/` | A reusable method: `README.md` (what it is + which engagements used it), `spec.md` (the method itself), and any supporting model (e.g. `model.xlsx`, built per [`docs/standards/excel-formatting.md`](./docs/standards/excel-formatting.md)). |
| `data/` | Sourced inputs, each with provenance (where it came from, as of when). |
| `analysis/` | Findings. `analysis/figures/` holds the charts those findings reference. |
| `docs/templates/` | Reusable document templates (e.g. `spec-template.md`) for drafting new specs. |
| `docs/standards/` | Repo-wide build standards (e.g. Excel formatting conventions). |
| `prompt-log.md` | Running record of AI sessions that produced a real artifact or decision. |
| `BIO.md` / `RESUME.md` | Reference material for voice and background. |

An **engagement** is one pass through the cycle: a brief defines the question, a capability (existing or new) supplies the method and model, analysis produces findings, and a decision closes it out with a recommendation. Capabilities are meant to outlive any single engagement — if a method proves reusable, it stays in `capabilities/` for the next engagement to pick up.

## Engagement index

| Engagement | Capability | Brief | Decision | Status |
|---|---|---|---|---|
| [perfect-competition](./docs/briefs/perfect-competition-brief.md) | [marginal-analysis](./capabilities/marginal-analysis/) | [brief](./docs/briefs/perfect-competition-brief.md) | — | Committed |
