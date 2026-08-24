# Excel Formatting Standards

**Purpose:** This file defines how any `model.xlsx` in this repo should be built —
by me or by an AI agent working from a `spec.md`. Reference this file from
`AGENTS.md` / `CLAUDE.md` and from each capability's `spec.md` so builds stay
consistent across Case 1, 2, and 3.

## Structure & Layout

- Each workbook has a clear entry point — a `Summary` or `Output` tab first,
  followed by `Inputs`, then `Calculations`.
- Tab names are short and specific (e.g. `Inputs`, `MC_Curve`, `Summary`) —
  never `Sheet1`, `Sheet2`.
- Freeze the header row on any tab with a data table.
- One workbook = one capability. Don't merge unrelated analyses into the same
  file.

## Formulas & Values

- **Formulas over hardcodes.** Any value that can be derived must be a
  formula. Only raw, exogenous inputs (given in the case prompt) are typed as
  constants.
- **Named ranges for drivers.** Key inputs and assumptions get a named range
  (`Marginal_Cost`, not a bare `$D$19` reference buried in a formula).
  Formulas should read close to plain English:
  `=Price - Marginal_Cost` rather than `=B4-D19`.
- No hardcoded numbers embedded *inside* a formula (e.g. `=D19*1.08`) — the
  `1.08` belongs in a named input cell, not typed into the formula itself.

## Visual Conventions: Editable Inputs vs. Formulas

The single most important distinction in any workbook: a cell you're meant to
type into, versus a cell that calculates something and should never be
hand-edited. Make this distinction impossible to miss and hard to violate by
accident.

| Cell type | Font color | Fill | Locked? |
|---|---|---|---|
| Editable input | Blue | Light yellow or gray | No |
| Formula / calculation | Black | White / none | Yes |
| Link to another tab or external source | Green | None | Yes |
| Flag / check / error | Red text or fill | — | Yes |

**Rule of thumb:** if you can trace a cell's value back to at least one raw
input through a chain of formulas, it's black and locked. If a cell's value
came from typing it in — a given price, a case assumption, a driver you're
testing — it's blue and unlocked.

### Enforcing it, not just color-coding it

Color alone gets overwritten by accident. Lock it down:

1. Select the entire sheet (Ctrl+A) → `Format Cells` → `Protection` tab →
   make sure **Locked** is checked. (This is the default for every cell, but
   confirm it.)
2. Select only the input cells (the ones that should stay blue/editable) →
   `Format Cells` → `Protection` → **uncheck** Locked.
3. `Review` → `Protect Sheet`. Now formula cells physically can't be typed
   into without unprotecting the sheet first — a hard stop, not just a
   visual hint.
4. Apply the font color (blue for unlocked/input, default black for
   everything else) as the last step, so it visually matches what's actually
   locked.

This means a reviewer — or you, six weeks later — can tell what's safe to
change just by glancing at the color, and can't accidentally break a formula
even if they try.

## Documentation

- Every workbook includes a `Notes` tab: what the model does, key
  assumptions, data sources, date built, and which case/session it supports.
- Every number has a visible unit ($, %, units/week, etc.) — no bare digits.

## Formatting Consistency

- Prices/currency: 2 decimal places, `$` format.
- Quantities: whole numbers unless the case specifically calls for
  fractional units.
- Rates/elasticities: percentage or 2–3 decimal places, applied consistently
  within a tab.
- Dates: `YYYY-MM-DD` to match the FRED/EIA/JODI source data conventions used
  elsewhere in the course.

## Auditability

- Where possible, include a simple check cell (e.g., a balance that should
  net to zero, or a total that should tie to a known figure) so an error is
  visible at a glance rather than buried.
- No hidden rows, columns, or sheets without a note in `Notes` explaining why.

## File Naming & Location

- Path: `capabilities/<capability>/model.xlsx`
- Accompanied by `capabilities/<capability>/spec.md` (the blueprint) and
  `capabilities/<capability>/README.md` (what it is, how to use it).
- Any AI-assisted build against this file gets logged in `prompt-log.md`.
