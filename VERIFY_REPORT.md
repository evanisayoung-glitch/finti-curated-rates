# Vivid Windows curated rate sheet — verification

**Result: PASS** on checks A–E.

Verified 2026-09-30 against:

- Attached sheet `Vivid-Windows-Curated-Rate-Sheet` (959 lines)
- Attached `build-summary.json` (updated 2026-09-29T21:05:00-05:00, `remy_403: true`)
- Attached `over16-mdr.json`
- `vivid-windows.html` on `main` of `evanisayoung-glitch/finti-curated-rates` at `a03e98e10ace70605d517537034996989aab467d`

Standing used for the math:

| Rule | Value |
| --- | --- |
| Typical Foundation risk | T3 6.5%, T4 8.5%, T1/T2 = 0 |
| Max approved dealer fee / MDR | 16% |
| Foundation metric | all-in = base MDR + Typical risk |
| Service Finance / Sunlight metric | dealer fee |
| Waterfall | Service Finance **or** Sunlight first look (toggle); Foundation always second; Sunlight never third |

Tolerance for all-in and risk: 0.1 percentage point. Parsed 28 plan rows (9 Service Finance, 3 Sunlight, 16 Foundation).

| Check | Result |
| --- | --- |
| A. Foundation Typical risk and all-in | **PASS** |
| B. Rows over 16% marked red / `over-mdr` / OVER 16% MDR | **PASS** |
| C. Banner states 16% max and Typical risk | **PASS** |
| D. Codes and fees not invented; no Enterprise / silo lecture | **PASS** |
| E. Mobile term-card CSS still shows APR, term, and fee | **PASS** |
| `main` `vivid-windows.html` vs attached HTML (over-MDR marking) | **MATCH** |

## A. Foundation Typical risk and all-in — PASS

Every Foundation row uses the standing risk for its tier. Displayed all-in equals base + that risk (exact, well inside 0.1). FREE base and an em dash risk are treated as 0.

| Code | Tier | Base | Risk | All-in | Base + risk |
| --- | --- | --- | --- | --- | --- |
| 84083 | 1 | 14 | 0 | 14 | 14 |
| 81229 | 2 | 1.5 | 0 | 1.5 | 1.5 |
| 82243 | 2 | 4 | 0 | 4 | 4 |
| 83101 | 2 | 13.5 | 0 | 13.5 | 13.5 |
| 80771 | 3 | 0 (FREE) | 6.5 | 6.5 | 6.5 |
| 80389 | 3 | 2.5 | 6.5 | 9 | 9 |
| 84061 | 3 | 6 | 6.5 | 12.5 | 12.5 |
| 80718 | 3 | 7.5 | 6.5 | 14 | 14 |
| 83908 | 3 | 10 | 6.5 | 16.5 | 16.5 |
| 83422 | 3 | 13.5 | 6.5 | 20 | 20 |
| 81828 | 3 | 16 | 6.5 | 22.5 | 22.5 |
| 81572 | 4 | 2.5 | 8.5 | 11 | 11 |
| 81145 | 1 | 0 (FREE) | 0 | 0 | 0 |
| 81751 | 1 | 14 | 0 | 14 | 14 |
| 81434 | 2 | 1.5 | 0 | 1.5 | 1.5 |
| 80484 | 2 | 13.5 | 0 | 13.5 | 13.5 |

No Foundation row failed tier risk or the all-in sum. The same 16 codes, tiers, bases, risks, and all-ins are in `build-summary.json` `ff_plans`.

## B. Over 16% marking — PASS

Metric: Foundation all-in, Service Finance / Sunlight dealer fee. Red means row class `over-mdr` (cell background `#FEF2F2`, fee/all-in/APR/risk color `#B91C1C` / `rgb(185, 28, 28)`) plus the `OVER 16% MDR` tag.

Expected codes are present and marked. No other sheet row is over 16%. No false positives.

| Code | Metric | Value | `over-mdr` | Tag | Red in Chrome |
| --- | --- | --- | --- | --- | --- |
| 83908 | all-in | 16.5 | yes | OVER 16% MDR | yes |
| 83422 | all-in | 20 | yes | OVER 16% MDR | yes |
| 81828 | all-in | 22.5 | yes | OVER 16% MDR | yes |

Missed overs: none.  
False positives: none.  
Other overs on this sheet: none.

Closest rows still at or under 16%, correctly unmarked:

- Sunlight 89708 dealer fee 14.99%
- Service Finance 3048 dealer fee 14.5%
- Foundation 80718, 84083, and 81751 all-in 14%
- Foundation 81828 base MDR is exactly 16%; the over mark is the 22.5% all-in, which is the Foundation metric

`over16-mdr.json` `account_over_portal` lists a Sunlight HVAC row at 17.99% (6.99% / 240 mo). That row is not on this Standard sheet (no `17.99`, no code `89210`). `curated_over_codes` and `build-summary.json` `over_mdr_codes` are the same three codes.

Chrome 148 at 1280px: the three rows use tag background `rgb(220, 38, 38)` and fee color `rgb(185, 28, 28)`. At 390px the same three cards use row background `rgb(254, 242, 242)`.

## C. Banner — PASS

`.over-mdr-banner` reads:

> Approved max dealer fee / MDR: 16%. Foundation all-in uses Typical risk (T3 6.5% / T4 8.5%; T1/T2 no risk). Rows in red are on this sheet and would put Vivid over 16% all-in at that risk — do not assign / pull from account if live.

That is the 16% max and the Typical risk copy (T3 6.5, T4 8.5, T1/T2 no risk). The same risk line also appears in the lede, playbook note, Foundation label, Foundation footnote, and footer.

## D. Codes and fees not invented; no Enterprise / silo lecture — PASS

Every sheet row matches `build-summary.json` on code, section, LGS plan name, APR, term, fee, and min–max. Foundation rows also match tier, risk, and all-in. No sheet code is missing from the summary, and no summary plan is missing from the sheet.

Visible copy has no “Enterprise”, “silo”, “Phase 1”, or HVAC lecture. The only HVAC lines are exclusion notes: “HVAC rows excluded” and “All HVAC-labeled rows excluded (Standard-lock).” Omitted Free Plan rows, unmatched Service Finance `*-63` rows, and Sunlight deferred 21.49% / 20.99% promos are not given product codes on the sheet.

## E. Mobile term cards — PASS

Inside `@media (max-width:720px)`, the term-card rules still label and show APR, term, and fee. The hide rule excludes `.apr`, `.term`, `.fee`, `.risk`, and `.allin`.

Chrome 148, viewport 390px, computed style:

| Surface | APR | Term | Fee label | Fee shown |
| --- | --- | --- | --- | --- |
| Service Finance 54101 | `::before` “APR”, display flex, 8.95% | “Term”, 180 | “Dealer fee”, 5.6% | yes |
| Foundation 83908 / 83422 / 81828 | “APR”, display flex | “Term”, 120 | “Base MDR” (Foundation override of “Dealer fee”) | 10% / 13.5% / 16% |

At 1280px those cells stay table cells (`::before` content `none`), so the mobile labels do not leak onto desktop.

## `main` vs attached HTML — MATCH

`origin/main:vivid-windows.html` (`a03e98e`) and the attached HTML are byte-identical.

SHA-256: `cd32c0ab8d9dddce305666cd1d9e595c776eec4425aae586a48bd66035b0b4ef`

Over-MDR marking on `main` is the same three rows (83908, 83422, 81828), the same `over-mdr` class, the same `OVER 16% MDR` tag, and the same banner. No drift.

## Standing waterfall (not A–E) — PASS

Confirmed on the same file so the sheet still matches the waterfall standing:

- First-look control is a tablist. Default is Service Finance (`body data-first="sfc"`).
- CSS hides the inactive first look. Foundation sits outside both panels and stays visible.
- Copy says Foundation is always second and Sunlight is never a third look, including both toggle states.

## Note (does not change A–E)

The Foundation footnote says “† marks all-in above 20%.” The dagger is on three all-ins:

| Code | All-in | `build-summary` `over20` | Above 20% |
| --- | --- | --- | --- |
| 83908 | 16.5% † | false | no |
| 83422 | 20% † | false | no (20 is not above 20) |
| 81828 | 22.5% † | true | yes |

`allin_over_20_kept` lists only 81828. The 16% red treatment on all three is still correct. The dagger footnote does not match 83908 or 83422.

Live Remy was not re-scraped. `build-summary.json` still has `remy_403: true`.
