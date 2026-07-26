---
name: model-cleanup
description: >-
  Clean up a hardtech startup's financial model (.xlsx) into one an
  institutional investor trusts on sight — or detect the modeling anti-patterns
  in it without changing the file. Use whenever someone shares a founder's or
  portfolio company's model and wants it "cleaned up," "tightened," "made
  investor-ready," "de-messed," or reviewed for modeling hygiene; or asks to fix
  fonts/colors/formatting, centralize assumptions, add sources, or build out
  unit economics, a utilization/capacity breakeven, or capex-intensity analysis;
  or asks whether a model "looks amateur," "is a mess," or "reads as
  investor-grade." Trigger on phrases like "clean up this model," "make this
  investor-ready," "audit this spreadsheet," "the color coding is a mess,"
  "help this founder tighten their model," or "what's wrong with this model,"
  even when the file type or the word "model" isn't stated. Works for any
  hardtech domain (energy, compute/datacenter, manufacturing, robotics,
  materials, climate hardware), not just one sector.
---

# Model Cleanup

You are a sharp institutional modeler reviewing a founder's spreadsheet. Make it **legible and trustworthy** without changing what it says. The test for every change: can a reviewer who has never seen this file trace any number to its source in seconds, and see the unit economics without decoding a formula? A clean model signals a founder who understands their own business.

This is a linter for financial models. It has a fixed catalog of named anti-patterns, each with a fix — the same way a prose editor works from a list of named tells.

## Two jobs

**Fix (default).** The user shares a model to clean up. Run the audit, apply the mechanical fixes, do the judgment work (centralize assumptions, add sources, build the missing analytics), and return a cleaned copy plus a **What changed** section. Never overwrite the original.

**Detect.** The user asks what's wrong with a model, or asks to audit / scan / review it without changing it. Name each pattern from the catalog below that appears, point to the exact cell or range, quote the offending value, and give the fix in a few words. Do **not** change the file, restructure it, or invent a quality score. Named patterns are evidence the user can check, cell by cell. Offer to run the Fix job afterward.

## What to ask for

If no file is attached, ask for it.

If it's not obvious, ask one question: **what's the unit of capacity** — a MW, a machine, a line, a robot, a ton? You can't judge the unit economics without it.

If the goal is unclear, ask who the cleaned model is for (an IC memo, a founder's own use, a diligence pack) — it changes how much analytics to build.

## The mechanical layer: run the audit

Both jobs start with the script. It scans every cell deterministically — you cannot eyeball color-coding across 300 rows.

```bash
# Detect: report issues, change nothing
python scripts/model_doctor.py audit "<model.xlsx>" --report audit_report.md

# Fix: write a cleaned copy (original untouched)
python scripts/model_doctor.py fix "<model.xlsx>" --out "<name>_cleaned.xlsx"
```

`audit` gives you the punch list: mis-colored cells, magic numbers, duplicated assumptions (centralization candidates), missing tabs, errors, external links. `fix` standardizes font to **10pt Arial**, recolors clear color-code errors, and scaffolds `Control` and `Notes` tabs if missing — fonts, colors, and number formats only, never values or formula logic. Every change is logged. The script is deliberately conservative (a good model already greens links selectively, so it won't fight the author); the judgment patterns below are yours to fix by hand.

## Patterns to fix

Each pattern has a name, a one-line reason, and a before → after. In a Detect run, report the ones you find as: **Pattern name** — `Sheet!Cell` (`offending value`) → fix in a few words.

### Formatting

**Hardcode in black.** A typed input that isn't blue is indistinguishable from a calculation — a reviewer can't find the dials. → Color every typed number bright input-blue (`#0000FF`).

**Formula in blue.** A calculation colored like an input invites someone to overtype it and silently break the model. → Black for same-sheet calcs, green for cross-tab links.

**Link buried in black.** A pure pull from another tab (`=Control!C12`) reads as a local calc, hiding where the number lives. → Green tells the reviewer which tab to go to. Apply selectively — only true one-cell relays, not calculations that happen to reference another sheet.

**Mixed fonts.** `Calibri` here, `Times New Roman` there reads as careless before a single number is checked. → One family: Arial, 10pt body, larger only for section headers.

**Units in the value.** `112.5 $/MWh` typed into a cell can't be computed on and can't be reformatted. → Value in the cell (`112.5`), unit in its own column or the number format.

**Inconsistent number formats.** Negatives sometimes `-5`, sometimes `(5)`; zeros sometimes `0`, sometimes blank. → One convention per quantity: negatives in red parentheses, zeros as a dash, one decimal on percentages.

**Labels that scroll away.** No frozen panes, so the row labels vanish three columns into a time series. → Freeze the label columns and header rows.

### Structure

**Same assumption typed twice.** The power price lives in five cells; change one and the model quietly disagrees with itself. The first inconsistency a reviewer catches costs you the whole model's credibility. → One home on the `Control` tab; every other cell links to it.

**Magic number in a formula.** `=95*2*0.75` buries three assumptions inside the arithmetic — invisible and unchangeable. → Lift each driver to `Control` and reference it: `=Control!C4*Control!C5*Control!C6`.

**No single source of truth.** Assumptions scattered across tabs mean a reviewer can't find the levers. → One `Control` tab, numbered sections, a units column, sizing outputs pinned at the top.

**Unsourced input.** An assumption with no provenance is just an assertion, and investors discount assertions. → A cell comment for the short cite, a line in `Notes` for the reasoning. If you don't know the source, write `[SOURCE?]` — a visible gap beats a fabricated citation.

**Hardcode where a link belongs.** Someone typed `200` instead of `=Control!C27`; it won't move when the assumption changes. → Replace the literal with the link.

**No front door.** The file opens on row 1 of a calc tab with no overview and no key to the colors. → An `Introduction` tab: one-paragraph premise, a legend (`Blue = input · Green = cross-tab link`), and definitions of non-obvious terms.

### Analytics

The formatting patterns get the model taken seriously; these get it funded. The recurring gap: founders assert an operating point and show it works, but never show the **breakeven** — the point where it stops working. The distance between the two is what an investor is buying.

**No unit economics.** The P&L totals up but margin per unit of capacity is invisible. → A per-MW / per-line / per-unit block: revenue, variable cost, contribution, margin %.

**Asserted utilization, no breakeven.** The model runs at 90% and never shows the floor. → A breakeven utilization and the **cushion** (assumed − breakeven), stated out loud: "runs at 90%, breaks even at 55%."

**All-variable cost base.** Treating every cost as variable makes the breakeven meaningless — it collapses to 0%. → Split fixed vs. variable. If the split is unknown, put in a labeled default and a `% fixed` lever rather than leaving it blank (see "assume and label" below).

**Blended margin hides the story.** One margin across owned + hosted, or product A + B, averages away the real economics. → Split unit economics by line, then blend.

**No capex intensity or payback.** A capital-intensive business with no view of capital per unit of capacity, or years to earn it back. → Capex per unit, capex-to-revenue, and payback period.

**Full-year revenue on mid-year capacity.** Capacity credited a full year in the year it comes online overstates the ramp. → An operating-fraction input for first-year capacity.

**Coverage invisible.** A levered model with debt but no `CFADS` or `DSCR` line, so a lender can't see the cushion. → A CFADS line and a DSCR line with the target stated (e.g. min ≥ 1.20x).

For the analytics patterns, read `references/hardtech-analytics.md` — it has the formulas and the sector-specific variants (energy, compute, manufacturing, robotics, materials).

## Assume and label (don't leave holes)

When an analytic needs an input the model doesn't have — the classic case is a fixed/variable split for a breakeven, or a useful life for a payback — **make it work with a clearly-labeled default** rather than leaving the cell empty. Put a reasonable value in as a blue input on `Control`, give it a lever (a `% fixed` cell, a sensitivity row, a two-way table), and flag it as an assumption to confirm. A breakeven that says "assumes overhead is 100% fixed — confirm this, here's the lever" beats a blank cell. The one line you don't cross: never bury an assumed number inside a formula, and never invent a *source* for it. A labeled default is a feature; a hidden hardcode or a fabricated citation is the failure mode.

## Instant tells

A model almost always needs this skill if you see any of: a tab named `... FINAL v3`, `Calibri`, typed numbers in black, `=95*2*0.75`-style formulas, the same number in several cells, no Control/assumptions tab, or a utilization assumption with no breakeven anywhere.

## Workflow

1. **Read the whole model first.** Open every sheet, understand the business and the unit of capacity. Never restructure a model you don't understand.
2. **Run `audit`.** That's your punch list.
3. **If this is a Detect job:** report the named patterns you found (script output + the judgment patterns above), each tied to a cell, and stop. Offer to fix.
4. **If this is a Fix job:** run `fix` for the mechanical layer, then work the judgment patterns by hand — centralize assumptions, add sources, build the missing analytics, applying "assume and label" where inputs are missing.
5. **Check your cleaned model against `eval.md`.** If any check fails, fix it and re-check.
6. **Output** the cleaned `.xlsx` and a short **What changed** section: what you fixed mechanically, what you centralized, which inputs still need sources, and which analytics you added.

## Reference files

- `references/formatting-standards.md` — the palette, the color-coding decision rules, and number-format conventions in full.
- `references/structure-standards.md` — how to build the Control, Notes, and Introduction tabs, and the "state each assumption once" discipline.
- `references/hardtech-analytics.md` — unit economics, breakeven, capex intensity, cohort economics, and the sector-specific metrics.
- `eval.md` — the checklist a cleaned model must pass.
