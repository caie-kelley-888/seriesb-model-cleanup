# Hardtech Analytics

Formatting gets a model taken seriously. This layer is what gets the company funded. Hardtech businesses — energy, compute/datacenter, manufacturing, robotics, materials, climate hardware — live or die on a few things a software model never has to face: they buy expensive physical capacity up front, that capacity earns only when it's utilized, and the whole thing is financed with debt against contracted cash. An investor reads the model to find out whether the unit economics work, how sensitive they are to utilization, and how long the capital takes to earn back. Build those views explicitly so they don't have to reverse-engineer them.

The recurring gap in founder models: they assert an operating point (a utilization, a price, a throughput) and show that it works. They rarely show the **breakeven** — the point at which it stops working. Investors care most about the distance between the two. Your highest-value additions almost always live here.

## 1. Unit economics per unit of capacity

Pick the natural unit of capacity for the business and express economics per unit, so margin is visible without decoding the full P&L. The unit varies by domain:

- **Compute / datacenter:** per MW IT (or per GPU, per rack).
- **Energy / generation:** per MW, per MWh.
- **Manufacturing:** per line, per unit shipped, per kg/ton of output.
- **Robotics / equipment:** per machine, per deployed robot, per robot-hour.
- **Materials / process:** per ton, per unit of throughput.

For that unit, build a clean stack:

```
Revenue per unit           (price × throughput × utilization × time)
– Variable cost per unit    (inputs, energy, consumables, maintenance)
= Contribution per unit
– Allocated fixed / opex
= Operating margin per unit
÷ Capex per unit
= Return on invested capital per unit
```

Show **gross margin %** and **contribution margin** explicitly — a reviewer wants the ratio, not just the dollars. If the business has multiple product lines or capacity types (owned vs. hosted, product A vs. B), show unit economics for each; blended margins hide the story.

## 2. Utilization / capacity breakeven — the most important addition

Every capital-intensive business has a utilization below which a unit doesn't cover its costs. Founders assume a utilization (often optimistically). Investors want to know the cushion. Build these explicitly:

- **Breakeven utilization:** the utilization at which contribution = fixed + financing cost per unit (or at which the target return / DSCR = 1.0). Solve for it, don't just eyeball it.
- **The cushion:** assumed utilization minus breakeven utilization. This one number frames the downside. "We run at 90% and break even at 55%" is a fundable sentence; the model should produce it.
- **Sensitivity:** margin, return, and coverage across a utilization range (e.g. 40%–100%). A small two-way table (utilization × price, or utilization × cost) is worth more than paragraphs.

The same logic generalizes beyond utilization: capacity-factor breakeven for energy, yield/throughput breakeven for manufacturing, occupancy breakeven for anything leased. Whatever the operational lever that scales revenue, find the level at which the unit stops paying for itself.

## 3. Capex intensity and payback

Hardtech is defined by the cash it sinks into capacity before earning. Make that legible:

- **Capex per unit of capacity** ($/MW, $/line, $/machine), broken into components where it matters (in datacenter: chip vs. datacenter shell vs. generation; in manufacturing: equipment vs. tooling vs. facility).
- **Payback period** — years of unit-level cash flow to earn back the unit's capex.
- **Capex intensity ratio** — capex per dollar of annual revenue or steady-state EBITDA. This is how an investor compares capital efficiency across very different businesses.
- **Maintenance vs. growth capex** split, and the **refresh/replacement cycle** (useful life). In fast-moving hardware, a clean model ties useful life, depreciation, debt tenor, and offtake term together rather than picking them independently.

## 4. Cohort / vintage economics

When the business builds capacity in waves — clusters, fabs, plants, fleets — model a single cohort's full lifecycle (build → ramp → steady-state → refresh/retire), then compound cohorts across the buildout. This does two things: it proves the unit works in isolation before scale flatters it, and it exposes the ramp — the mid-life period where new capacity is online but not yet fully utilized. A common honest touch is an **operating-fraction** input for capacity in its first (partial) year, so revenue isn't credited for a full year on something that came online mid-year.

## 5. Financing and coverage (for debt-financed models)

Capital-intensive businesses are usually levered, and the debt structure is part of the unit economics, not an afterthought:

- **CFADS** (cash flow available for debt service) as a clean line — revenue net of opex, before financing.
- **DSCR** (debt service coverage ratio), ideally a small taxonomy of labeled lines answering different questions (project-level vs. consolidated, lender-basis vs. all-in). State the target (e.g. min DSCR ≥ 1.20x) and show whether it's met throughout.
- **Debt sizing by asset/stack** — different assets support different leverage; size each rather than applying one blanket LTV.
- **Reserves** (debt-service and O&M reserve accounts) where lenders require them, so early-year cash isn't overstated.
- **Prepayments / offtake** — contracted customer cash that de-risks the build should be modeled explicitly, since it's often the reason the economics close.

## 6. Valuation anchor

Where relevant, anchor terminal / enterprise value on a capacity-based comp (EV per MW, per unit of capacity, per ton of output) benchmarked against public or transaction comps, then subtract net debt to reach equity. Show a **sum-of-the-parts asset floor** alongside as a downside anchor. Keep the exit multiple conservative and say so — returns that rest on a peak-cycle re-rate don't survive diligence.

## How to use this

Don't dump all of this into every model — read the business and add what's missing or thin. In practice the highest-leverage moves are almost always: (1) a clean per-unit economics block, (2) a real utilization/capacity breakeven with the cushion stated, and (3) capex intensity + payback.

Build the structure and wire the inputs to the Control tab. When an analytic needs an input the model doesn't have — the classic case is a **fixed/variable cost split** for a breakeven, or a useful life / financing tenor for a payback — don't leave the analysis empty. Put in a **reasonable, clearly-labeled default** as a blue input, add a lever so the founder can flex it (a "% fixed" cell, a sensitivity row, a two-way table), and flag it prominently as an assumption to confirm. A working breakeven that says "assumes labor+overhead is 100% fixed — confirm this, here's the lever" is far more useful to a founder than a blank cell, *as long as the assumption is visible and easy to change.* The line you don't cross: never bury an assumed number inside a formula, and never invent a *source* for it. A labeled default is a feature; a hidden hardcode or a fabricated citation is the failure mode.
