# AGENTS.md

Conventions for any AI agent (Claude, Codex, Cursor, etc.) working in this repo. This is Jennifer's personal working repo for strategy/transformation work — not a software project — so treat these as working conventions for research, analysis, and drafting, not build/test instructions.

## What this repo is for

A place to run engagements through a repeatable cycle, build reusable analytical capabilities, and keep a record of what AI helped with. If you're not sure where something belongs, ask rather than guessing a new top-level folder.

## Structure and where things belong

- `capabilities/<name>/` — a reusable method: `README.md` (what it is + which engagements used it), `spec.md` (the method itself), and any supporting model file (e.g. `model.xlsx`). If you build or refine a reusable method during analysis, promote it here rather than leaving it buried in `analysis/`.
- `data/` — sourced inputs. Every dataset needs provenance (where it came from, as of when). Don't add data you can't source.
- `analysis/` — findings. `analysis/figures/` — the charts those findings reference.
- `docs/briefs/` — written **before** work starts: scope + hypothesis. Check for one before doing substantive analysis; write one if it doesn't exist.
- `docs/decisions/` — written **after** work finishes: the recommendation, addressed to a specific audience.
- `prompt-log.md` — running record of AI sessions that mattered. Add an entry (what was asked, what changed) after any session that produced a real artifact or decision — not routine back-and-forth.
- `BIO.md` / `RESUME.md` — reference material for voice and background. Treat as source of truth for tone when drafting anything in Jennifer's voice; propose edits to these files rather than changing them silently.
- `.claude/skills/` — Claude-specific skills, if any are added.

## Confidentiality — never paste into a model

- **Client/employer-identifying data**: Servco-specific figures, internal names, vendor names, or anything else that identifies a specific engagement or organization. Generalize or anonymize before it goes into a prompt.
- **Personal/HR data**: names or specifics about coworkers, direct reports, or board members (performance, roles, conflicts, etc.). Refer to roles or functions, not people.
- If a piece of data is ambiguous, ask before including it rather than assuming it's fine.

## Drafting style

- Default to structure, not prose: propose an outline, headers, or bullet points and let Jennifer write the actual sentences — unless the prompt explicitly asks for a full draft.
- When a full draft is explicitly requested (a brief, a decision doc, a resume bullet, etc.), write it in full, match the voice established in `BIO.md`/`RESUME.md`, and mark it clearly as an AI draft until it's been reviewed.
- Never merge a draft into `RESUME.md` or `BIO.md` directly — hand it back for review first.

## Explanations

Default to concise, direct answers. Expand into a fuller walkthrough only when asked, or when the reasoning behind a recommendation is itself the deliverable (e.g. a `docs/decisions/` entry).

## Nested AGENTS.md

If a subfolder later gets its own `AGENTS.md` (e.g. inside a specific `capabilities/<name>/`), it takes precedence over this file for anything inside that folder.
