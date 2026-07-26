# model-cleanup

A linter for hardtech financial models. Point it at a founder's or portfolio company's model and it either **cleans it up** into something an institutional investor trusts on sight, or **detects** the modeling anti-patterns in it without changing the file.

Inspired by the [no-ai-slop](https://github.com/petergyang/no-ai-slop) skill — same idea, applied to spreadsheets instead of prose: a fixed catalog of named anti-patterns, each with a fix.

## Two jobs

- **Fix (default).** Audits the model, applies the mechanical fixes (font, color code, number formats), centralizes assumptions, adds sources, builds the missing analytics, and returns a cleaned copy plus a *What changed* section. The original is never overwritten.
- **Detect.** Names each anti-pattern that appears, points to the exact cell, quotes the value, and gives the fix in a few words — without touching the file.

## What it checks

Twenty named patterns across three groups:

- **Formatting** — hardcodes in black, formulas colored like inputs, mixed fonts, units typed into values, inconsistent number formats.
- **Structure** — the same assumption typed in five places, magic numbers buried in formulas, no single source of truth, unsourced inputs, no legend.
- **Analytics** — no unit economics, an asserted utilization with no breakeven, blended margins that hide the story, no capex intensity or payback.

The house color code: inputs **bright blue `#0000FF`**, same-sheet formulas **black**, cross-tab links **bright green `#00B050`**, external/warning **red `#FF0000`**. Body font Arial 10pt.

Works across hardtech domains — energy, compute/datacenter, manufacturing, robotics, materials, climate hardware.

## Contents

- `SKILL.md` — the skill: two jobs, the pattern catalog, the workflow.
- `eval.md` — the checklist a cleaned model must pass.
- `scripts/model_doctor.py` — the audit/fix engine (`audit` and `fix` subcommands; requires `openpyxl`).
- `references/` — the formatting, structure, and hardtech-analytics standards in full.

## Install

Download the repo and add it as a skill in Claude (Cowork → Skills → add), or drop the folder into your skills directory. Then just say things like "clean up this model" or "what's wrong with this model?" and attach the `.xlsx`.
