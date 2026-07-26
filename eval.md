# Eval — does the cleaned model pass?

Check the cleaned workbook against every item. If any fails, fix it and re-check. This is the evaluator pass of the loop, the same way a prose editor checks a draft against a rubric.

## Did you break anything? (non-negotiable)

- **Original untouched.** The source file is unchanged; the cleaned copy is a new file.
- **Values preserved.** Every computed result matches the original (recalc and spot-check the headline numbers — revenue, gross profit, capex, key returns). Cleaning changes formatting and where inputs live, never the answers.
- **No new errors.** No `#REF!`, `#DIV/0!`, or broken links introduced by the rewiring.

## Formatting

- **One font.** Arial throughout; 10pt body, larger only for headers. No Calibri / Times survivors.
- **Color code holds.** Typed inputs are bright blue `#0000FF`; same-sheet calcs black; cross-tab links bright green `#00B050`; external/error red. No formula colored like an input.
- **Number formats consistent.** One convention per quantity; negatives in red parentheses, zeros as a dash, one decimal on percentages.
- **Units are separate.** No `112.5 $/MWh` typed into a value cell; units live in their own column or the format.
- **Labels stay visible.** Frozen panes keep row labels and headers on screen.

## Structure

- **Single source of truth.** Every assumption lives once, on the `Control` tab. No number is typed in two places.
- **No magic numbers.** No business driver buried inside a formula (`=95*2*0.75`); each is a link to `Control`.
- **Sources present.** Each `Control` input has a comment or a `Notes` line, or an honest `[SOURCE?]`. No fabricated citation.
- **Front door.** An `Introduction` tab with a one-line premise, the color legend, and definitions.

## Analytics

- **Unit economics visible.** A per-unit-of-capacity block: revenue, variable cost, contribution, margin %.
- **Breakeven with cushion.** A utilization/capacity breakeven, and the cushion (assumed − breakeven) stated in words.
- **Fixed/variable split exists** where a breakeven is claimed (an all-variable base makes it meaningless). A labeled default + lever is fine if the real split is unknown.
- **Capex intensity + payback** for a capital-intensive business.
- **Assumptions, not holes.** Every value the analysis needs is present — as a real number or a clearly-labeled default with a lever — never a blank cell or a buried hardcode.

## Deliverable

- **What changed section** written: mechanical fixes, what was centralized, inputs still needing sources, analytics added.
