# Vivid Windows curated rate sheet — verification

Verified file: `Vivid-Windows-Curated-Rate-Sheet.html` (final modular HTML, attached 2026-09-30).
Sidecars in this folder: updated `build-summary.json` (`modular: true`, 2026-09-29 21:05 CT) and the earlier `plans_included.md` for Service Finance and Foundation names.
Live Remy / FPM was not queried. `remy_403` is still the expected state.

## Executive summary

Evan, this file passes the checklist.

The tab control is on the page and it works. **First look (choose one)** is a tablist: Service Finance or Sunlight. Picking one hides the other table. Foundation stays on the sheet in both states, labeled second look. The playbook and the step cards follow the same choice: step 1 is the lender you picked, step 2 is Foundation, and Sunlight is never framed as a third look.

Service Finance is still the nine pack codes. Foundation is still the sixteen pack codes, with **81828** marked **22.5% †**. Sunlight is only **87125, 89708, 86860**, and those fees match the updated build summary. No Free Plan row and no unmatched Service Finance `*-63` row was added. Logos are embedded and readable on the navy header.

**Overall: PASS.**

The earlier interim file `Vivid-Windows-Curated-Rate-Sheet.fixed.html` is removed. The attached sheet is the one under test, and it already has the toggle.

## Checklist

| Check | Result | Severity | What we saw |
| --- | --- | --- | --- |
| A. Waterfall copy: SFC or Sunlight first, Foundation second, no third look | **PASS** | — | Lede, playbook, two-step cards, and footer. Default copy: first look Service Finance, Foundation always second, never run Sunlight as a third look. After choosing Sunlight, the lede, playbook, and step-1 card switch to Sunlight and still say Foundation is second and Sunlight is not a third look. “Third look” appears only to rule that framing out. |
| B. Modular toggle, mutually exclusive; Foundation always shown | **PASS** | — | `role="tablist"` with two tabs. Chrome: Service Finance selected shows `#panel-sfc` and sets `#panel-sunlight` to `hidden` / `display: none`. Sunlight selected does the reverse. `#foundation` stays `display: block` and `hidden: false` in both states, including after scrolling to it and at a 390px width. Left arrow returns to Service Finance. Reload keeps the last choice. Print CSS hides the control and the inactive first look; it does not hide Foundation. |
| C. Service Finance codes match `plans_included.md` | **PASS** | — | Exactly `54101, 54126, 54626, 54649, 52012, 4089, 4102, 2024, 3048`. APR, term, dealer fee, and LGS name match the pack and `build-summary.json`. Non-Counter badge is on the five Non-Counter rows. |
| D. Foundation codes match `plans_included.md` | **PASS** | — | All 16 codes, once each. APR, term, base, risk, all-in, and tier match the pack. See the 81145 label note below. |
| E. 81828 † / 22.5% flagged if present | **PASS** | — | Row prints `22.5% †` with the over-cap style. The footnote says the dagger means all-in above 20% and to confirm before quoting. Visible in Chrome under the Sunlight first look. |
| F. FLAG-omit Free Plan and unmatched SFC `*-63` not invented | **PASS** | — | No “Free Plan” text. No `Standard - 1099/1199/1249/1399/1449 - 63`. The two extra omit rows in the new build summary (Sunlight deferred 21.49% / 84 and 20.99% / 84) are not plan rows. HVAC code 89210 is not on the sheet. |
| G. Self-contained HTML; logos OK on light wells | **PASS** | — | Finti and Vivid logos are the same inline PNGs as the previous sheet. Finti (dark wordmark) sits on a white well. Vivid is a black badge with a light wordmark on a `#0B0B0B` well, and it stays readable on the navy header at desktop and at 390px. IBM Plex Sans loads from Google Fonts, with system fonts as the fallback. |
| H. No Enterprise / silo / HVAC lecture on the Standard page | **PASS** | — | No “Enterprise”, “silo”, or “Phase 1”. No HVAC plan rows and no second Sunlight sheet. Two short lines say HVAC-labeled rows were left off. |

## Sunlight rows

Checked against `build-summary.json` → `sunlight_plans`. `plans_included.md` is the earlier pack and still does not list these three codes. The HTML matches the updated summary. Fee `0.0` is printed as `FREE`.

| Code | Plan name | APR | Term | Dealer fee | Min – Max |
| --- | --- | --- | --- | --- | --- |
| 87125 | Fixed APR of 11.99% for 180 months | 11.99% | 180 | FREE | $5,000–$100,000 |
| 89708 | Fixed APR of 6.99% for 180 months | 6.99% | 180 | 14.99% | $5,000–$100,000 |
| 86860 | Fixed APR of 6.99% for 144 months | 6.99% | 144 | 12.49% | $5,000–$100,000 |

Product rows are unchanged from the previous HTML (28 codes, same cells). The difference is the tab control, the dynamic playbook, and the two-step waterfall.

Header badges follow the active first look: Service Finance selected shows “9 Service Finance” and hides “3 Sunlight”; Sunlight selected shows “3 Sunlight” and hides “9 Service Finance”. “16 Foundation” stays. Both counts are in the file.

## Notes that did not fail a check

- **81145 all-in label.** Base MDR prints `FREE`. All-in prints `0%`. `plans_included.md` labels that all-in `FREE`. `build-summary.json` stores `allin: 0.0`. The code and the zero fee are right. Low.
- **`plans_included.md` lag.** It still says every Sunlight row was excluded. The updated build summary is the sidecar that lists 87125, 89708, and 86860. Info.
- **Internal words.** “CERTAIN” appears on the Sunlight step card. “Standard-lock”, “Ultimate Brain”, and `remy_403` remain in footnotes. `remy_403` is expected. They are not Enterprise / silo / Phase 1. Low.
- **External font.** Tables and logos render from the file itself. The webfont request is the only network dependency. Info.

## How this was checked

- Product rows diffed against the previously verified HTML: identical.
- Service Finance and Foundation cells compared with `plans_included.md` and `build-summary.json`. Sunlight cells compared with `sunlight_plans` in the updated summary.
- Omit portal names from `flags_omit` and `sunlight_omitted` searched in the HTML. None of those rows are on the sheet.
- Chrome, fresh profile, desktop 1280px: toggle visible (71px tall), both tabs clickable, panels mutually exclusive, Foundation height stayed 1342px and visible in both states. ArrowLeft from Sunlight returned to Service Finance. Reload restored the saved choice.
- Chrome at 390px: both tabs fit on one line (no horizontal overflow). Tap on Sunlight hid Service Finance and left Foundation shown.
