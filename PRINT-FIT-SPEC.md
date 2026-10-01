# US Letter print fit

Curated Finti rate sheets (Service Finance, Foundation, Sunlight, HVAC, dealer waterfalls) print from Chrome to US Letter without clipping a column, a product name, or a Min–Max amount.

Stylesheet: `print-fit.css`  
Exemplar: `exemplar-print-fit.html` (same classes, rates copied from the live sheets)

## Page

| | |
|---|---|
| Size | US Letter, portrait (`8.5in × 11in`) |
| `@page` margin | **0.45in** on all sides |
| Fit band | 0.40in–0.50in. Column percentages are budgeted against a **7.5in** content box (the 0.50in case), so 0.40in and 0.45in have slack. |
| Content width at 0.45in | 7.60in |
| Content width at 0.50in | 7.50in |

Do not add a second margin in the print dialog. The stylesheet owns the margin. Chrome’s header/footer is off (`--no-pdf-header-footer`) so it does not steal the bottom of the last row.

## What was clipping

Screen CSS sets `table { min-width: 720px }` inside `.scroll { overflow-x: auto }` and `.table-card { overflow: hidden }`, with `.wrap` padding of 24px. On Letter that scrollport is narrower than the table, so the last column is cut to `Min – M` and `$1,000–$100,00`. `td.range { white-space: nowrap }` keeps the amount on one line, which is correct only when the column itself is on the page. Product cells were squeezed in the same overflow.

Print CSS turns overflow visible, drops the min-width, and uses `table-layout: fixed` with the percentages below.

## Type

| Role | Size | Behavior |
|---|---|---|
| Sheet title (`h1`) | 13pt | Compact navy header |
| Lede | 8.5pt | |
| Column headers | 7pt | Wrap inside the header cell. `thead` repeats (`table-header-group`). `position: sticky` is cleared so it does not pin or clip. |
| Product name (`td.name`) | 8pt | Wraps on spaces inside the cell (`overflow-wrap: break-word`). |
| Plan code, APR, term, fee, Min–Max | 7.5pt | `white-space: nowrap`. Tabular numerals. |
| Type / tier / program badges | 6.25pt | Nowrap. Column is wide enough for `NON-COUNTER`. |
| Body navy header padding | 8px 0 10px | Logo well 28px tall |

Rows do not split: `tr`, `th`, and `td` use `break-inside: avoid`. Sections and table cards may break between rows so a long sheet is not forced onto one page and then clipped.

## Column widths

Percent of the table. Each profile sums to 100. Min–Max is 17% or 18% (~1.28–1.35in at a 7.5in content box), which holds `$25,001–$100,000` at 7.5pt.

### `table.rate-sfc` — Service Finance (7)

Plan # 8 · Product 34 · Type 14 · APR 9 · Term (mo) 8 · Dealer Fee 10 · **Min – Max 17**

### `table.rate-sl` — Sunlight (7)

Code 9 · Product 40 · APR 9 · Term (mo) 8 · Promo 8 · Dealer Fee 9 · **Min – Max 17**

### `table.rate-ff` — Foundation (8)

Code 8 · Product 32 · APR 8 · Term (mo) 8 · Promo 7 · Dealer Fee 10 · Tier 9 · **Min – Max 18**

### `table.rate-hvac` — Foundation HVAC (9)

Code 8 · Product 22 · Program 12 · APR 8 · Term (mo) 7 · Promo 7 · Dealer Fee 9 · Tier 9 · **Min – Max 18**

### `table.rate-vivid-sfc` — dealer waterfall, Service Finance (8)

Product Code 9 · Plan 26 · APR 8 · Term (mo) 7 · Dealer fee 8 · **Min – Max 17** · Type 12 · Why we use it 13

### `table.rate-vivid-sl` — dealer waterfall, Sunlight (7)

Product Code 10 · Plan 32 · APR 8 · Term (mo) 8 · Dealer fee 9 · **Min – Max 17** · Why we use it 16

### `table.rate-vivid-ff` — dealer waterfall, Foundation (10)

Product Code 8 · Plan 16 · APR 7 · Term (mo) 6 · Base MDR 7 · Risk 6 · All-in 7 · Tier 7 · **Min – Max 18** · Eligibility 18

Live sheets:

| File | Table class |
|---|---|
| `sfc.html` | `rate-sfc` |
| `sunlight.html` | `rate-sl` |
| `foundation.html` | `rate-ff` |
| `hvac.html` (both tables) | `rate-hvac` |
| `vivid-windows.html` | `rate-vivid-sfc`, `rate-vivid-sl`, `rate-vivid-ff` |

`print-fit.css` is linked with `media="print"` after each sheet’s `<style>` block, so screen layout is unchanged. Keep the stylesheet next to the HTML. A pack that writes HTML into a subfolder has to copy `print-fit.css` beside those files or rewrite the `href`.

## Chrome print-to-PDF

```bash
google-chrome \
  --headless \
  --disable-gpu \
  --no-pdf-header-footer \
  --no-first-run \
  --disable-extensions \
  --hide-scrollbars \
  --virtual-time-budget=20000 \
  --print-to-pdf=out.pdf \
  exemplar-print-fit.html
```

Pass the HTML as a `file://` URL (`--print-to-pdf` writes the PDF path you give it). Do not set a custom paper size or margin on the command line; `@page` does. `--virtual-time-budget` is optional. With it set, headless Chrome can keep running after the PDF is on disk; the file is complete once Chrome logs the byte count. These flags were checked on Letter output for the exemplar and for `sfc.html`, `foundation.html`, `sunlight.html`, `hvac.html`, and `vivid-windows.html`.

Check:

```bash
pdfinfo out.pdf          # Page size 612 x 792 pts (Letter)
pdftotext -layout out.pdf -
```

The extract must contain the full header `Min – Max`, full product names (for example `12 Months, Zero Interest with NO Monthly Payments- Same as Cash`), and full amounts (`$1,000–$100,000`, `$25,001–$100,000`). A second page of a multi-page sheet repeats `Min – Max` in the header row.

## Rates

Numbers in the exemplar are copied from `sfc.html`, `foundation.html`, `sunlight.html`, and `hvac.html`. Nothing in this fit pass changes a rate, a code, or a product name.
