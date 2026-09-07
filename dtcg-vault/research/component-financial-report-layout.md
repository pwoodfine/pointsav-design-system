# Research — Financial Report Layout

This file records why each decision in the financial-report-layout component
was made. A generation agent producing a proforma, income statement, book
valuation, or summary sheet should read this, then emit HTML/CSS that honours
the constraints.

## 1. Why `table-layout:fixed` + explicit label `width:25%`

**Problem:** A compliance financial report is several separate `<table>`
elements (Revenue, Costs, Capital, Returns), each with its own section heading
and page-break behaviour. With default `table-layout:auto`, each table sizes
columns to its own content, so "Y3" in the Revenue table lands at a different
x-position than "Y3" in the Costs table. The period alignment — the entire
point of a multi-year statement — is lost.

`table-layout:fixed` makes column widths a function of CSS rules, not cell
contents. Every `table.wide` resolves the same widths: label at 25%, the
remaining 75% divided across 11 data columns. Because the rule set is shared,
every wide table gets identical geometry and columns align without per-table
tuning.

The rule is **restated inside `@media print`** because some print engines
re-resolve table layout at print time and would otherwise revert to auto,
breaking alignment in the PDF.

**Codegen rule:** keep column count and label/data split identical across every
`table.wide` in the same aligned group. A document may hold more than one
aligned group (e.g. four 8-column per-entity statements plus a separate
11-column multi-year build-out). `table.wide` is the right tool for any set
of same-shaped statements that should read as a uniform stack.

## 2. Why `tr.total` / `tr.subtotal` / `tr.section-banner` as semantic classes

These are **roles**, not styles. Encoding them as classes (not inline):

1. Single source of visual truth — one edit changes every total.
2. Machine-readable — a tool can find every total with `tr.total`.
3. Non-colour differentiation — roles escalate by border weight and
   font-weight, not just fill, surviving greyscale print and colour vision
   deficiency.

**Codegen rule:** apply exactly one role class per emphasised row, on the
`<tr>`. Banner rows use `colspan="12"` over the 12 authored content columns.

## 3. Why `td.lnum` is injected by JS, not server-rendered

Line numbers are a property of the *rendered document*, not the *data*. Baking
them into source rows requires recomputation every time a row is added, removed,
or reordered. Client-side injection after layout means the number is always
correct for the document as rendered, continuous across all tables.

The `!important` flags on `.lnum` exist solely to win against the inherited
`tr.total` / `tr.subtotal` / `th` backgrounds and weights — the gutter must
read as a margin rule, not as a figure.

**Codegen rule:** emit data rows only. Append the injection script once at the
end of `<body>`. Do not author `.lnum` cells. Do not count the gutter in your
`colspan` values.

## 4. Why letter-landscape `@page`

A label plus 11 periods does not fit portrait at a legible size. Landscape is
the compliance print standard for multi-year statements. **Letter** (not A4)
because the audience is a North American securities context (BCSC); the
regulator and filers print on letter stock.

Margins `1.5cm 2cm 1.5cm 1.5cm` — the wider value on the bound/punch edge
leaves room for holes without eating the gutter or first data column.

**Codegen rule:** do not change `@page` for a compliance financial report.

## 7. Why `.page-break-before` / `.page-break-after` as utility classes

Multi-statement forecasts routinely need individual statements on their own
pages (e.g. Per-DHS rollup and Reconciliation each on a fresh page). The base
component keeps tables whole (`break-inside:avoid`) and headings glued to
their tables (`break-after:avoid`), but has no way to force a statement onto a
new page.

`.page-break-before{break-before:page;page-break-before:always}` applied to
a `<h2>` does this. It composes correctly with `h2,h3{break-after:avoid}` —
the heading stays glued to its note + table at the top of the new page.

**Codegen rule:** add `.page-break-before` to the `<h2>` of any statement
that must not share a page with the preceding section. Use sparingly —
a forced break applied too liberally creates near-empty pages by orphaning a
preceding section's trailing note. Do not use `transform:scale()` to fit
content: Chrome computes page breaks on layout dimensions, not visual size.

## 5. Colour tokens and semantic meaning (SUPERSEDED — see §8 for the V5 canonical values)

> The table below documents the original delivered proforma (pre-V4). It is kept
> for historical record only — every value it names was replaced by the V5
> canonical spec (§8). Do not use this table for new work.

| Value | Used by | Meaning |
|---|---|---|
| `#111` | body text | Primary ink |
| `#555` | `p`, `p.note` | Secondary ink — narrative subordinate to tables |
| `#333` | `h3` | Sub-heading ink |
| `#ccc` | borders, `h2` rule | Hairline grid |
| `#f5f5f5` | `th` background | Header band |
| `#aaa` / `#bbb` | `td.lnum` ink (screen / print) | Gutter numerals — faint, recedes |
| `#d0d0d0` / `#ccc` | gutter right border | Boundary: gutter / content |
| `#eef2f7` | `tr.total` fill | Heaviest emphasis. Bottom line |
| `#f5f7fa` | `tr.subtotal` fill | Lighter emphasis. Intermediate sum |
| `#e3edf7` | `tr.section-banner` fill | Most saturated. Names a block |
| `#1a2a44` | `tr.section-banner` ink | Dark navy — the only coloured text |
| `#888` | `tr.total` top border | Heavy rule above bottom line |
| `#aaa` | `tr.subtotal` top border | Lighter rule above intermediate sum |
| `#666` / `#ddd` | `.footer` ink / rule | Compliance notice, quietest block |

The three emphasis fills are one cool-blue family at three saturations — the
hierarchy reads as one coherent system. All three clear WCAG contrast against
`#111` / `#1a2a44` in print.

**Codegen rule:** do not recolour a role. If a brand theme overrides, keep the
three-saturation relationship intact.

## 6. Typography scale (SUPERSEDED — see §8 for the V5 canonical scale)

> Same status as §5 — historical record of the pre-V4 delivered document only.

| Element | Size | Rationale |
|---|---|---|
| `body` | 13px (11px print) | Base; steps down in print to fit landscape |
| `h1` | 1.25rem | Document title; one per document |
| `h2` | 1rem + hairline rule | Statement section |
| `h3` | 0.9rem | Sub-section, no rule |
| `p` | 0.82rem | Narrative — smaller than base, subordinate to tables |
| `p.note` | 0.78rem italic | Inline caveat |
| `table` | 0.76rem (10px print wide) | Smallest legible; maximises columns per page |
| `tr.section-banner` | 0.74rem uppercase | Header read at low height via uppercase + letter-spacing |
| `td.lnum` | 9px monospace | Below data size; reads as metadata |
| `.footer` | 0.72rem | Required but visually deprioritised |

`system-ui` throughout (no web-font dependency); `'Courier New'` monospace
on the gutter reads as a ruled margin.

**Codegen rule:** do not enlarge `p`; do not shrink table type below 0.76rem
screen / 10px print — figures clip below that with 13 columns on letter landscape.

## Research trail

### Done (10)
- Extracted CSS, line-number JS, and HTML patterns verbatim from the delivered
  Client B V2 proforma (primary source; polished over two sessions).
- Verified cross-table alignment depends on `table-layout:fixed` + shared 25%
  label width, and that the rule must be restated in `@media print`.
- Confirmed `!important` on `.lnum` is required to override inherited
  total/subtotal/header backgrounds.
- Confirmed the three emphasis fills are one blue family at three saturations
  and clear WCAG contrast against their text in print.
- Confirmed `print-color-adjust:exact` is required for gutter and fills to
  survive print.
- Confirmed colspan accounting: 12 authored content columns; gutter inserted
  by script outside the authored colspan.
- Validated WeasyPrint 61+ as non-Chromium print engine (Building Portfolio V2,
  2026-06-13): `@page` letter-landscape, `break-before:page`, `table-layout:fixed`
  cross-table alignment, and all semantic-row fills all render correctly. JS
  line-number gutter absent (WeasyPrint does not execute JavaScript). Use
  Chromium when line numbers are required; WeasyPrint for line-number-optional
  drafts and CI. `print-color-adjust:exact` warning logged but harmless.
- Validated white-space-eliminating flow pagination (SPV Partnership reference build,
  2026-06-21): forcing every section atomic on a landscape proforma where
  sections are shorter than the page strands a too-tall statement on its own
  page, leaving the page above it half-empty. Tagging the single tallest
  statement `section.block.tall` (allowed to flow across pages at row
  boundaries) while keeping all other sections atomic eliminated the gaps —
  4 half-empty pages → 3 full pages.
- Validated the `.masthead` + absolutely-pinned `.draft` stamp pattern (same
  source): the prior floated-stamp pattern let a long description wrap under
  the stamp; `position:relative` masthead + `position:absolute` stamp removes
  the stamp from flow entirely, so it cannot overlap regardless of description
  length.
- Confirmed `td.tbd` (muted `•` glyph) is the correct treatment for a figure
  not yet known — distinct from a blank cell (reads as nil to a reviewer) and
  from a fabricated placeholder number. Keeps the row and line number legible;
  excluded from computed subtotals/totals by convention, stated in the
  section's `p.note`.

### Suggested (2)
- Validate the letter-landscape margin asymmetry against a real binding/
  hole-punch sample.
- Two dashboard-theme token candidates surfaced (`engine.finance.draft.{ink,size,weight}`,
  `engine.finance.tbd.{ink,glyph}`) — not a DTCG change yet, per source; fold into
  the engine-finance-bundle on a future design pass, not registered here.

### Open questions (1)
- Should `td.lnum` be semantically addressable (e.g. an `id` per line) so a
  reviewer's "line 42" can deep-link, or does it remain purely decorative?

**Resolved (2026-06-21):** repeating `<thead>` for tables taller than one
printed page. Author each table with a real `<thead>`/`<tbody>` split and add
`thead{display:table-header-group}` in print — this reprints the header row
on every page the table spans. Confirmed it does **not** disturb the
line-number injector: the injector runs once at load and numbers rows in
document order, and `table-header-group` is a paint-time repeat of the same
header row, not an extra DOM row.

## 8. V5 canonical spec (2026-07-29) — supersedes §5/§6, and the JS gutter in §3

A senior design/typography audit (rendered and inspected the whole live proforma
family) drove a full formatting pass, applied to the real engine renderer
(the canonical V5 renderer) and verified in WeasyPrint across the family: SPV1
2→1pp, Management 2→1pp, ShareCapital 4→3pp (nested-flexbox pie overlap fixed),
Commissions 15→8pp — one visual system, no dead-space orphans.

**Font.** Carlito (Calibri-metric), not `system-ui` — `system-ui` is a silent
WeasyPrint no-op that falls back to DejaVu Sans (wide, heavy), never the
intended font. Render host needs `fonts-crosextra-carlito` installed.
`font-variant-numeric:tabular-nums lining-nums` is mandatory on every cell —
Carlito's default figures are proportional and would break column alignment
without it.

**Horizontal rules only — the single highest-impact change.** No vertical cell
borders. `th,td{border:0;border-bottom:1px solid #e3e3e3}`. Header = 1.5px
bottom rule, no fill. Subtotal = single 1px top rule, no fill. Total = double
rule (2px top + 2px bottom), no fill. `tr.section-banner` is the **only**
filled row (`#f2f4f7`, plain ink — the old saturated `#e3edf7` fill and its
dedicated navy `#1a2a44` ink are both retired). This directly replaces every
value in §5's table.

**Type scale — explicit px, no rem/px mixture:** h1 17px / h2 13px / h3 11.5px
/ h4 10.5px / body 10px / note 9.5px italic / table 10px. Directly replaces
§6's table. `h1` also carries `string-set:doctitle content()` for the running
header.

**Line-number gutter is server-rendered, not JS-injected (corrects §3).**
WeasyPrint does not execute JavaScript, so a client-side `.lnum` injector never
reaches the primary PDF render target — the entire point of the compliance
line-number column was silently lost in every WeasyPrint render. Emit `lnum`
cells at generation time instead: a doc-wide running counter prepends
`<td class="lnum">` (or `<th>` for header rows) to every table row at HTML
write time, skipping layout-table rows. Hand-authored docs bake the cells in
once. §3's "why JS, not server-rendered" rationale is retracted — the
*reasoning* about correctness-after-reorder still holds for a build step that
runs at generation time, just not for a runtime client-side script when the
render target doesn't run JS at all.

**Accounting negatives in parentheses** — `($1.54M)`, `(19K)`, `$(0.77)` — a
formatter-level rule (`fmt_*` helpers), not CSS. `—` (em-dash) for nil, never
blank or `0`. **Currency symbol:** plain `$`, stated exactly once near the
title (a single "All amounts CAD" masthead line) — not per-figure, not in
table titles. (This reverses a same-day back-and-forth: a `CAD `-per-figure
convention was tried and reverted within the same session; treat `$`-once as
settled, not provisional.)

**No flexbox for print.** WeasyPrint overlaps nested flex containers onto
adjacent content — confirmed on the ShareCapital pie-chart-beside-table
layout. Use `table.layout` (a single-row layout table that zeroes out the
global `th,td` styling) or `display:inline-block` with explicit widths for any
chart-beside-table or two-up band.

**Pagination.** `section.block{break-inside:avoid}` wraps every
heading+note+table+footnote group so they paginate as a unit. Default every
section atomic; the single tallest statement in a document may flow instead
(rows stay intact via `tr{break-inside:avoid}`, header reprints via §
"Resolved" above) so a preceding short section's page fills rather than
stranding white space. Reserve forced `page-break-before` for statements that
must legally/visually start on a fresh page — over-using it orphans small
tables alone on otherwise-empty pages.

**Running header/footer.** `@top-left` shows the doctitle (from `string-set` on
`h1`), `@top-right` shows a draft-stamp string, `@bottom-center` shows
`counter(page) " / " counter(pages)` — all suppressed on `@page :first`.
`@page{margin:1.05cm 1.1cm 1.2cm 1.1cm}` (asymmetric: bottom kept larger for
the page-number line).

**Codegen rule:** any new document built on this component uses the V5 values
above, not §5/§6's original figures. The engine `HEAD` const and this
component's canonical CSS must stay byte-identical on font/rule/type-scale/
`@page` rules — a divergence is drift to close (see §10), not a variant.

## 9. Reusable content patterns (V8, 2026-07-23)

Three prose/composition patterns from a real compliance-revision session,
distinct from the CSS/token rules above — these live in `usage.md` variant
guidance, not as new DTCG tokens (they are compositional signals, not visual
constants).

- **Optional-overlay section spacing.** An optional/alternate-scenario section
  (restates a base section under a different assumption) gets an inline
  `style="margin-top:14px"` on its own `section.block` — a per-document signal,
  not a shared class change.
- **External-reference note.** When a computed line depends on a mechanism
  documented in full elsewhere, add one `p.note` immediately after the table
  naming the source document and giving only the one-sentence consequence — do
  not inline the source document's own tables or year-by-year figures; that
  duplication is exactly what creates drift between the two documents later.
- **Forward-computed fixed-sum disclosure.** Where a fee/rebate is a fixed sum
  (not derived backward from a target net), say so explicitly and state what
  it is *not* conditioned on — update the prose's causal direction whenever a
  figure changes from "solved backward from a net target" to "a fixed gross
  figure computed forward."

## 10. Family drift audit (V8, 2026-07-23) — corrects an earlier "drift closed" claim

An earlier pass (2026-07-16) stated the engine `HEAD` now matched this
component's canonical CSS and treated any future divergence as drift to close.
That was true for the canonical V5 renderer only. A full audit of every
HTML-rendering report file in the proforma engine's report-rendering directory found the
canonical CSS reaching **one of nine** files in what should be one component
family:

| File | Disposition |
|---|---|
| the canonical V5 renderer | **Matches V5 canonical** — the reference this whole spec is written from. |
| `legacy_jv_proforma.rs`, `alloc_client_c_proforma.rs`, `client_d_proforma.rs`, `client_b_proforma.rs`, `building_portfolio_v2.rs`, `client_a_forecast_v1.rs` | **Diverged — pre-V4.** Each carries its own separate copy of the old head/CSS: `system-ui`, full bordered grid, tinted fills, JS-injected gutter, 1.5cm/2cm margins. `building_portfolio_v2.rs`'s own `HEAD` comment literally claims sync with this component and is wrong. |
| `d1_dev_classes_v2.rs` | **Diverged — structurally, not just stylistically.** Different HTML pattern (`td.r`/`td.grp`, no `tr.section-banner`), no line-number gutter at all, 1280px max-width. Closer to a variant than a drifted copy. |
| `client_d_sensitivity_v7.rs`, `client_d_sensitivity_v8.rs`, `tearsheet_alt_re_v2.rs` | **Not this component.** Interactive Chart.js dashboards — a different, screen-first product, correctly out of scope. |

**Live defect, not fixed in this token landing:** the six pre-V4 files are
shipping the exact bug V5 fixed — `system-ui` silently falls back to DejaVu
Sans under WeasyPrint, and the JS-injected gutter never reaches a
WeasyPrint-rendered PDF at all. Any compliance document currently generated
via those code paths is, right now, rendering in the wrong font and silently
missing its line-number gutter whenever produced through WeasyPrint. Design
artifact only this round (operator decision) — see
`proforma-reproducibility.md` for the follow-up.

**Also noted, not a drift target:** `forecast_statements.rs` (in
project-proforma's separate top-level proforma-engine tool, not the
`pointsav-monorepo/` sub-clone every file above lives in) implements a
genuinely different portrait/serif "classic statement" look, aligned with this
design system's own `financial-statement-yearend` component rather than this
one.

### Research-trail delta (V4–V8)
- Done +7: font-is-a-no-op finding + Carlito install; horizontal-rules table
  system; explicit px type scale; accounting-parentheses formatter;
  flexbox→table.layout pie-overlap fix; server-rendered line-number gutter
  (retracts §3's JS-injection rationale for the WeasyPrint target); tighter
  `@page` margins + row density + anti-dead-space pagination (no forced breaks
  on small sections).
- Done +3: three reusable content patterns catalogued (§9).
- Done +1 / correction: full nine-file family drift audit (§10) — the
  2026-07-16 "drift closed" claim narrowed to one file; six files identified
  still shipping the pre-V4 defect; one file identified as a structural
  variant; three files confirmed correctly out of scope.
- New hard finding: `forecast_statements.rs` is a third, legitimately distinct
  classic-statement family — not a drift target of this component.
- Open questions +1: should the six pre-V4 files be brought forward to V5
  canonical CSS? Scoped out of this pass by operator decision — see
  `proforma-reproducibility.md`.

## 11. Pagination-robustness scaffold — `.chapter`/`.chapter-opener`/`.bridge`/`.takeaway` break rules (2026-09-07, from project-proforma)

**Origin:** project-proforma's `tool-direct-hold-engine` (2026-08-13/22/23,
`BRIEF-direct-hold-engine-fold-consolidation.md`) built a numbered
chapter/exhibit scaffold for its Land-Parcel Flexibility document, then
(2026-08-27/28) ported the same break-rule primitives into this
`financial-report-layout` family as *additive, forward-compatible* rules —
validated empirically by rendering 5 of 13 live table-token documents to PDF
and confirming none currently need them (no live stranded-heading defect
found in that set). Landed here as a DESIGN-RESEARCH contribution rather than
silently kept local, since the mechanism is genuinely reusable: any
multi-section report using `table.wide`/`section.block` (§1–§2 above) can hit
the same class of pagination defect this scaffold prevents.

**The defect this prevents:** a content block (a chart card, a data box, a
captioned figure) split cleanly across a page boundary by WeasyPrint, because
nothing told the renderer to keep it whole. `section.block{break-inside:avoid}`
(§1) already protects whole `<table>` elements; it does not reach a `<div>`
sitting directly inside a `section.chapter` wrapper, or a standalone caption/
lead paragraph — that gap is what this scaffold closes.

**The rules, generalized (no chapter numbering or Woodfine-specific
styling — those are this document's own presentational choice, not part of
the reusable mechanism):**

```css
/* Chapter-boundary pagination */
.chapter{page-break-before:always}
.chapter.chapter-continuous{page-break-before:auto}   /* opt out for a chapter that should flow, not force-break */

/* Keep-together primitives -- apply to any block that must not split across a page */
.chapter-opener{break-inside:avoid;page-break-inside:avoid}
.bridge{break-before:avoid;page-break-before:avoid}    /* a transitional paragraph that must stay with what follows it */
.chapter-head,.chart-card,.databox,.methodbox,.scopebox,.confidential-callout,.cap,.lead{
  break-inside:avoid;page-break-inside:avoid
}

/* Heading-orphan protection, extending §7's existing utility-class pattern */
h2,h3,h4,.stitle{break-after:avoid;page-break-after:avoid}
```

**Codegen rule:** when a component introduces its own boxed/callout element
(a card, a captioned figure, a pull-quote — anything that reads as one visual
unit), add it to the `break-inside:avoid` keep-together list rather than
inventing a new rule per component. Chapter numbering (`.chapter`) is opt-in —
a document using flat `table.wide` sections without chapters never needs it.

**One rule intentionally left OUT of this generalization: `.takeaway`.** The
source implementation styles it as `border-left:4px solid #164679` with
`.takeaway strong{color:#164679}` — `#164679` is Woodfine's own brand blue
(confirmed against `woodfine-media-assets/token-global-color.yaml`'s
`woodfine-blue`), not a generic PointSav accent. The keep-together mechanics
(`break-before:avoid;page-break-before:avoid`, already covered by the
`.chapter-head,.chart-card,...` selector list above) are reusable; the accent
color is not. A consuming application using this callout pattern should
supply its own accent color (e.g. `border-left-color: var(--accent-primary,
#111111)`), not this literal hex. Per `.agent/rules/design-tokens.md`'s
tenant-neutrality rule (this archive), Woodfine's actual brand values stay in
`woodfine-media-assets`, not forked in here — flagged to project-proforma
separately rather than silently copied into this file's generic guidance.

### Research-trail delta (V8–V9)
- Done +1: pagination-robustness scaffold generalized from project-proforma's
  real production validation (5 of 13 live documents rendered to PDF, 0
  stranded-heading defects found with the scaffold applied).
- New finding: the source implementation's one color-bearing rule
  (`.takeaway`'s accent) hardcodes Woodfine's own brand blue rather than a
  generic accent — excluded from this generalization; documented as a
  tenant-substitution point instead.
- Open question: should a "Presentation" variant of this family (chart/
  bento-box handout style, scorecard/card/keymsg visual language — the other
  half of project-proforma's contribution, `PRESENTATION_PROFORMA_CSS`) be
  ratified as a new canonical Paper document family? Not resolved in this
  pass — that CSS's entire `:root` scheme is Woodfine brand color end to end
  (not just one rule), and minting new primitives for a new document family
  is a bigger design decision than this pass's scope. Routed back to
  project-design/operator as a follow-up, not decided here.
