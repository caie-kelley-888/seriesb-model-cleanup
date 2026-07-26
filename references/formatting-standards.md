# Formatting Standards

The point of formatting discipline is legibility and trust, not decoration. A reviewer should be able to glance at any cell and know instantly whether it's an input they can change, a calculation, or a value pulled from elsewhere. That is the entire job of the color code. Everything here serves that.

## Font

- **Arial, 10pt** for the model body. This is the house standard and matches how institutional models are built. (Some shops use 9pt; if the user tells you their standard is 9pt, honor it — the principle is consistency, not the specific size.)
- Section headers and titles can be larger (e.g. 11–15pt) and bold. Keep the hierarchy shallow and consistent: one size for the sheet title, one for section headers, one for the body.
- One font family throughout. A model with three fonts reads as careless before a single number is checked. If you find Calibri or Times mixed into an otherwise-Arial model, standardize it.

## Color coding — the core discipline

Font color encodes *where a number comes from*. This is the single most important convention in the whole model, because it lets a reviewer audit trust at a glance.

| Cell content | Color | Hex | Meaning |
|---|---|---|---|
| Hardcoded input (a typed number) | **Bright blue** | `#0000FF` | "You can change this. It's an assumption." |
| Formula referencing only the same sheet | **Black** | `#000000` | "This is calculated here." |
| Formula that's a link to another sheet | **Bright green** | `#00B050` | "This is pulled from elsewhere — go there to change it." |
| External link / warning / error | **Red** | `#FF0000` | "Danger — external dependency or broken reference." |
| Labels, notes, sub-captions | Black or gray | `#000000` / `#7F7F7F` | Not part of the code; gray for secondary text. |

These are the exact target colors. Use the bright, saturated blue (`#0000FF`) and green (`#00B050`) — the standard modeling convention — not the muted variants (`#0070C0`, forest green) some models drift into. The audit tolerates any recognizable blue/green shade so it won't nag a model that's close, but the fix normalizes everything to these exact values.

The mental model: **blue = you type it, black = it's computed here, green = it lives on another tab.** A reviewer scanning for "what drives this model" looks for blue. A reviewer tracing a number back to its origin follows green.

### The color heuristic (and its edge cases)

The auto-fix classifies each cell like this:

- **Numeric literal → blue.** A cell whose value is a typed number with no formula.
- **Formula with a cross-sheet reference and no real math → green.** A "link": `=Control!C27`, `=+Summary!C12`, or a simple pass-through (`=+SUM(TopCo!C57:Q57)` that only pulls one tab through). The signal is that the cell exists to *relay* a value, not compute one.
- **Formula that computes → black,** even if it references another sheet. `=Control!C17*C37` is a calculation, not a link. Black.
- **Text → left alone** (labels and headers keep their color).

Edge cases the heuristic can get wrong, which is why you spot-check:

- A formula that mixes a cross-tab pull with local math (`=Control!C47+C49`) is genuinely ambiguous. Convention leans black (it computes), but some modelers green it because the *inputs* are all links. Either is defensible; be consistent within the model.
- A hardcode that's really a placeholder for a link (someone typed `200` instead of `=Control!C27`) should become a green link — but the script can't know that. The audit flags duplicated literals so you can catch these.
- A "0" or "1" toggle or a switch flag is a hardcode (blue) but often intentionally local.

When unsure, prefer the reading that helps the reviewer, and keep the model internally consistent.

## Number formats

Consistency matters more than any specific format. Pick one convention per quantity type and apply it everywhere:

- **Currency / large numbers:** thousands separators, negatives in red parentheses, zero shown as a dash. E.g. `#,##0;[Red](#,##0);-` or `$#,##0;[Red]($#,##0);-`.
- **Percentages:** one decimal is usually right — `0.0%;[Red](0.0%);-`.
- **Multiples / ratios:** a fixed decimal, often with a label — `0.0"x"` or `0.0"x DSCR"`.
- **Units belong in their own column or in the number format, never mixed into the value.** Don't type `112.5 $/MWh` in a cell; put `112.5` in the value and `$/MWh` in the unit column.

Negatives as red parentheses and zeros as dashes are worth enforcing — they make a financial statement instantly scannable and are a quiet signal of a modeler who's done this before.

## Layout hygiene

- **Freeze panes** so row labels (usually columns A–B) and header rows stay visible when scrolling. A time-series model that scrolls the labels off-screen is exhausting to read.
- **A wide label column** (A) and a dedicated **units column** (B). Labels should be full sentences where needed, not cryptic abbreviations.
- **Consistent column structure** across time-series sheets — same year in the same column on every tab, so cross-tab links line up.
- **No stray formatting** — merged cells used sparingly, no random fills, no leftover highlighter from someone's edit session.
- **Section numbering** (1, 1a, 1b, 2a…) gives every block an address and makes the model navigable and referenceable in conversation.
