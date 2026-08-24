# Templates & Examples

Reusable templates for course materials, assignments, and professional portfolio materials. These provide consistent structure across all student work — the same way investment banks and consulting firms maintain firm-wide templates for models, memos, and pitch materials.

## What is a "spec"?

In the OpenAI video [Introducing Specs](https://openai.com/index/introducing-specs/), a spec (short for specification) is a structured document that clearly defines:

- the goal of a task
- the steps required
- the expected inputs/outputs
- the evaluation criteria

It acts as a contract between humans and AI systems, ensuring consistency, reproducibility, and clarity in how work is done.

### Specs vs. Prompts

**Prompts:** Instructions given to an AI in natural language. They are flexible, conversational, and immediate. Example: "Explain Interest Rate Parity with an example."

**Specs:** Formalized blueprints that outline how to approach a problem systematically. They provide context, requirements, and evaluation rules.

**How they complement each other:**

- A spec defines the scope and structure of a task.
- A prompt executes a part of that task inside the spec.

In practice:

- The spec ensures that multiple people (or agents) would approach the problem the same way.
- The prompts are the tactical instructions used at each stage.

### Using Specs + Prompts in Economics

**In teaching:** Specs define structured student projects (e.g., analyzing tariffs, calculating FX hedges). Prompts help students generate analysis, visuals, or summaries.

**In research:** Specs define methodology (e.g., data sources, models, reproducibility requirements). Prompts handle execution (e.g., regression code, lit review summaries).

**In policy/consulting:** Specs provide consistent evaluation frameworks (e.g., for assessing monetary policy). Prompts generate scenario narratives and what-if analyses.

**Together they enforce clarity, consistency, and reproducibility — exactly what employers expect.**

---

## Templates Directory

### Assignment & Project Templates

- **[`spec-template.md`](./spec-template.md)** — Technical specification (originally authored for ratios analysis, adaptable to other model-driven projects)

---

## Frontmatter Schema

Every Markdown template in this directory carries a YAML frontmatter block so humans and LLMs can identify the template's purpose without reading the body.

```yaml
---
template: spec                    # spec (add more values here as more template types are added)
purpose: "Short description of what the template is for"
audience: student                 # student | instructor | both
fields_required: [list, of, fields, the, student, must, fill, in]
naming_convention: "YYYY-MM-DD-{slug}.md"
courses: [BUS-314, BUS-629, FIN-321]   # optional — courses that use this template
notes: "Optional caveats or adaptation notes"
---
```

**Why frontmatter:** When a student (or an LLM in a spec-drafting workflow) is choosing which template to apply, they should not have to read the entire body to figure out what the template is for. The `purpose` and `fields_required` keys answer that question in one block.

---

## File Naming Conventions

A single naming convention applies across the repo. When in doubt, follow these:

| Artifact | Convention | Example |
|----------|------------|---------|
| Technical spec | `YYYY-MM-DD-{slug}.md` | `2026-05-15-aapl-ratios-spec.md` |

**Slug rules:** lowercase, hyphen-separated, no spaces or underscores. Keep slugs short but descriptive (3–6 words).

**Date rules:** ISO format (`YYYY-MM-DD`) so files sort chronologically. Use the date the document was first authored, not the date it was last edited.

---

## How to Use These Templates

1. **Copy the appropriate template** to your course or project directory
2. **Rename** following the naming convention above
3. **Customize for your needs** — Add course-specific context, requirements, and examples
4. **Keep the frontmatter** — Update fields like `courses` if you adapt the template, but leave the schema intact so tooling continues to work
5. **Maintain consistency** — Use the same template structure across all projects and courses

## Quick Reference

| Need | Template |
|------|----------|
| Create a project spec | [`spec-template.md`](./spec-template.md) |
