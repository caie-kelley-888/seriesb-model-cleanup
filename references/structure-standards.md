# Structure Standards

Formatting makes a model legible cell-by-cell. Structure makes it trustworthy as a whole. The three ideas here — one home for every assumption, a written record of where assumptions come from, and a front-door legend — are what separate a model a reviewer can audit from one they have to reverse-engineer.

## Rule 1: State every assumption exactly once

The most common failure in a founder's model is the same number typed in five places. When the founder later changes their power price, or chip cost, or throughput assumption, four of the five don't update, and the model quietly becomes internally inconsistent. A reviewer who spots one such inconsistency stops trusting all the numbers.

The fix: **every assumption lives once, in a dedicated `Control` tab, and every other cell links to it.** If a number is an input — something a human decided rather than the model computed — it belongs in Control, in blue, and nowhere else.

### How to build the Control tab

Lay it out so a reviewer can read the entire business model in one scroll:

- **Sizing outputs at the very top** — the handful of results the whole model exists to produce (raise size, key returns, min DSCR, breakeven). These link *down* into the model (green). Putting them first lets a reader see the answer before the assumptions.
- **Numbered sections below**, grouped by theme. From a real model: `1. Key inputs`, `2. Mix & utilization`, `3. Scale`, `4. Funding`, `5. Opex`, `6. Capex`, `7. Cost breakdown`, `8. Debt sizing`, `9. Debt terms`… Number them so every input has an address.
- **Three columns:** label (A, wide, plain English), unit (B, e.g. `$/MWh`, `%`, `months`, `$mm/MW`), value (C, the blue input or a green link to a deeper calc).
- **Inputs in blue, derived constants in black/green.** If a cell in Control is itself computed from other inputs (e.g. "capex per GPU = chip $/MW ÷ GPUs/MW"), that's fine — it's a derived constant, shown in black, kept in Control so the derivation is visible.

Then, everywhere else in the model, an assumption is `=Control!C17`, not a typed number. The audit script's "centralization candidates" list is your worklist for this.

### What stays local

Don't be dogmatic. A `0`/`1` switch, a `12` for months in a year, a genuinely one-off local constant — these can stay put. The test is: *would a reviewer want to find and change this?* If yes, it goes in Control. If it's plumbing, leave it.

## Rule 2: Source every assumption

An input without a source is just an assertion, and investors discount assertions. Two complementary places to record sourcing:

- **Cell comments** for the short version — attach a note to the input cell: `"Contingency + arranger/legal fees. Rates in Control."` or `"B300 share of revenue-equivalent MW; revenue weighting, not physical capacity."` Comments keep the sourcing next to the number without cluttering the grid.
- **A `Notes` (methodology) tab** for the reasoning — numbered sections mirroring the model, each a short paragraph explaining *why* an assumption is what it is, what convention was used, and what was deliberately simplified. This is where you defend the model: "Utilization runs at a conservative X% versus a Y% technical ceiling," "Debt amortization is modeled as level payments over tenor," "Generation capex is on an effective-MW basis."

If you don't know a source, write `[SOURCE?]` and move on. A visible gap is honest and gives the founder a punch list. A fabricated source is a landmine — the one time a reviewer checks it and it's wrong, the whole model is suspect.

## Rule 3: Give the model a front door

The first tab should orient a reader who has never seen the file:

- **A one-paragraph overview** of the business and the model's premise.
- **A legend** decoding the color scheme in one line: `Blue = input · Green = cross-tab link. Figures indicative, subject to diligence.` This is small but it tells a reviewer the model was built with a convention in mind.
- **Definitions** of any non-obvious terms and units (what's a "MW IT," a "CFADS," a "cohort").
- Optionally, **investment highlights** — the 4–6 things that make the business work, in plain language.

Each model sheet can also carry a one-line **premise** at the top ("Per-MW unit economics compounded across the buildout") so a reader knows what they're looking at before they read a single number.

## A note on restructuring

Centralizing assumptions means rewiring cells to point at Control. Do this carefully — a botched find-and-replace can break formulas. Work section by section, verify the model still balances after each block, and never change the *logic*, only *where the input lives*. If you're not confident a change preserves the result, flag it for the founder instead of making it.
