# Vivid Windows curated rate sheet — verification

Verified file: `Vivid-Windows-Curated-Rate-Sheet.html` (updated SoT attached after the Brain waterfall fix, 2026-09-30).
Compared with `plans_included.md` and `build-summary.json` from the same upload set.
Live Remy / FPM was not queried. `remy_403` is still the expected state.

## Executive summary

Evan, the updated sheet tells the waterfall correctly. Service Finance **or** Sunlight is first look. Foundation is always second. Sunlight is never cast as a third look. The Service Finance and Foundation codes match the pack (9 and 16). The three Sunlight home-improvement codes from the update are on the page: **87125, 89708, 86860**. No HVAC Sunlight table was added. Plan **81828** is marked **22.5% †**. The omitted Free Plan rows and the unmatched Service Finance `*-63` rows were not invented onto the sheet. Logos are embedded and readable.

The sheet still fails the modular viewer. There is no control. Service Finance and Sunlight both stay on screen, so a dealer cannot pick one first look and hide the other. That is check **B, FAIL, high**.

A sibling file, `Vivid-Windows-Curated-Rate-Sheet.fixed.html`, adds that control only. Every product row is copied from the source. No fee, APR, term, or code was added or edited. Use the fixed file if this sheet needs to ship with the toggle before the next build.

**Overall: FAIL** until the source has a working first-look toggle. Seven of eight checks pass.

## Checklist

| Check | Result | Severity | What we saw |
| --- | --- | --- | --- |
| A. Waterfall copy: SFC or Sunlight first, Foundation second, no third look | **PASS** | — | Lede, playbook, step cards, and footer say dealer picks Service Finance or Sunlight as first look and Foundation is always second. The words “third look” appear only to rule that framing out. Step numbers are 1, 1, then 2, joined by “or” and “then”. |
| B. Modular toggle, mutually exclusive; Foundation always shown | **FAIL** | High | No `<button>`, `<input>`, or `<script>`. Both `#service-finance` and `#sunlight` render together. Foundation is always in the page, which is right, and the choice itself is not enforced. |
| C. Service Finance codes match `plans_included.md` | **PASS** | — | Exactly `54101, 54126, 54626, 54649, 52012, 4089, 4102, 2024, 3048`. APR, term, dealer fee, and LGS name match the pack. Non-Counter badge is on the five Non-Counter rows. |
| D. Foundation codes match `plans_included.md` | **PASS** | — | All 16 codes are present, once each. APR, term, base, risk, all-in, and tier match the pack. See the 81145 label note below. |
| E. 81828 † / 22.5% flagged if present | **PASS** | — | Row prints `22.5% †` with the over-cap style. The footnote says the dagger means all-in above 20% and to confirm before quoting. |
| F. FLAG-omit Free Plan and unmatched SFC `*-63` not invented | **PASS** | — | No “Free Plan” text. No `Standard - 1099/1199/1249/1399/1449 - 63`. No extra Foundation or Service Finance codes. |
| G. Self-contained HTML; logos OK on light wells | **PASS** | — | Finti and Vivid logos are inline PNG data URIs. Finti (dark wordmark) sits on a white well. Vivid is a black badge with a light wordmark and sits on a `#0B0B0B` well on the navy header, so the wordmark stays visible. IBM Plex Sans is loaded from Google Fonts, with system fonts as the fallback. |
| H. No Enterprise / silo / HVAC lecture on the Standard page | **PASS** | — | No “Enterprise”, “silo”, or “Phase 1”. No HVAC plan rows and no second Sunlight sheet. Two short lines say HVAC-labeled rows were left off. |

## Sunlight rows on the updated HTML

`plans_included.md` and `build-summary.json` still say every Sunlight row was excluded as HVAC. They do not list 87125, 89708, or 86860. The follow-up SoT is the updated HTML, which adds these three Standard home-improvement plans and no others.

Printed on the sheet (not rewritten, not taken from a live portal):

| Code | Plan name on the sheet | APR | Term | Dealer fee | Min – Max |
| --- | --- | --- | --- | --- | --- |
| 87125 | Fixed APR of 11.99% for 180 months | 11.99% | 180 | FREE | $5,000–$100,000 |
| 89708 | Fixed APR of 6.99% for 180 months | 6.99% | 180 | 14.99% | $5,000–$100,000 |
| 86860 | Fixed APR of 6.99% for 144 months | 6.99% | 144 | 12.49% | $5,000–$100,000 |

Counts on the header match the page: 9 Service Finance, 3 Sunlight, 16 Foundation.

The footnote also says deferred 21.49% / 84 and 20.99% / 84 (fee 0) were omitted for lack of a Brain match. Those rows are not on the sheet, and no code was invented for them.

## Notes that did not fail a check

- **81145 all-in label.** Base MDR prints `FREE`. All-in prints `0%`. `plans_included.md` labels that all-in `FREE`. `build-summary.json` stores `allin: 0.0`. The code and the zero fee are right. The word on the all-in cell differs from the markdown label. Left as printed. Low.
- **Sidecar lag.** The markdown and JSON in this folder are the earlier pack. They still describe a Service Finance first look and “Sunlight HVAC excluded.” The HTML is ahead of those files. Info.
- **Internal words on a dealer page.** “CERTAIN”, “Standard-lock”, “Ultimate Brain”, and `remy_403` show up in dealer-facing copy. `remy_403` is expected. They are not Enterprise / silo / Phase 1. Low.
- **External font.** Opening the file offline still shows the tables and logos. The webfont request is the only network dependency. Info.

## Fix applied

`Vivid-Windows-Curated-Rate-Sheet.fixed.html`

- Segmented control under the playbook: Service Finance | Sunlight.
- Choosing one sets `hidden` on the other first-look section. Foundation is never hidden.
- The matching playbook card gets a green outline. Both playbook cards stay visible so the “or” rule remains on the page.
- Arrow keys move the choice. `#sunlight` or `#sl` opens on Sunlight.
- With scripts off, Sunlight starts hidden and the sheet stays on Service Finance, then Foundation.
- Diff of product rows against the source is empty (28 codes, same cells).

## How this was checked

- Code, APR, term, fee, and name cells read from the updated HTML and compared to `plans_included.md` and `build-summary.json`.
- Forbidden strings searched on the visible text (Enterprise, silo, Phase 1, Free Plan, the five `*-63` portal names, HVAC plan rows).
- Logos decoded from the data URIs and checked against the wells in CSS, then looked at in Chrome on the navy header. Finti reads on the white well. The Vivid badge reads on its dark well.
- Chrome opened the source file: zero buttons, zero scripts, and Service Finance, Sunlight, and Foundation all `display: block`.
- Chrome opened the fixed file and clicked the control. Service Finance selected: Sunlight section `hidden` and `display: none`; Service Finance and Foundation shown. Sunlight selected: Service Finance hidden; Sunlight shown with 87125, 89708, 86860; Foundation still shown underneath, including 81828. Clicking back restored Service Finance.
- Opening the fixed file at `#sunlight` starts on Sunlight with Foundation still shown. Left arrow from that control returns to Service Finance and hides Sunlight. Foundation stayed visible in every state.
- A 390px-wide viewport still showed the logos, the stacked playbook, and the same exclusive first look.

## Service Finance cell check (pack vs sheet)

| Code | Type | APR | Term | Fee | Sheet |
| --- | --- | --- | --- | --- | --- |
| 54101 | Non-Counter | 8.95% | 180 | 5.6% | Match |
| 54126 | Non-Counter | 8.95% | 120 | 5.1% | Match |
| 54626 | Non-Counter | 8.95% | 120 | 7.1% | Match |
| 54649 | Non-Counter | 12.95% | 120 | 4.1% | Match |
| 52012 | Non-Counter | 0% | 12 | 8.1% | Match |
| 4089 | Standard | 6.99% | 180 | 9.6% | Match |
| 4102 | Standard | 6.99% | 144 | 8.6% | Match |
| 2024 | Standard | 0% | 24 | 13% | Match |
| 3048 | Standard | 0% | 48 | 14.5% | Match |

## Foundation cell check (pack vs sheet)

| Code | Tier | APR | Term | Base | Risk | All-in | Sheet |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 84083 | T1 | 5.9% | 120 | 14% | — | 14% | Match |
| 83101 | T2 | 7.9% | 120 | 13.5% | — | 13.5% | Match |
| 82243 | T2 | 11.9% | 120 | 4% | — | 4% | Match |
| 81229 | T2 | 12.9% | 120 | 1.5% | — | 1.5% | Match |
| 81828 | T3 | 9.9% | 120 | 16% | 6.5% | 22.5% † | Match, flagged |
| 83422 | T3 | 10.9% | 120 | 13.5% | 6.5% | 20% | Match |
| 83908 | T3 | 11.9% | 120 | 10% | 6.5% | 16.5% | Match |
| 80718 | T3 | 12.9% | 120 | 7.5% | 6.5% | 14% | Match |
| 84061 | T3 | 13.5% | 120 | 6% | 6.5% | 12.5% | Match |
| 80389 | T3 | 14.9% | 120 | 2.5% | 6.5% | 9% | Match |
| 80771 | T3 | 15.99% | 120 | FREE | 6.5% | 6.5% | Match |
| 81572 | T4 | 16.99% | 144 | 2.5% | 8.5% | 11% | Match, min $7,501 |
| 81751 | T1 | 5.9% | 180 | 14% | — | 14% | Match, min $25,001 |
| 81145 | T1 | 11.9% | 180 | FREE | — | FREE in the pack; sheet all-in prints 0% | Code match; label note above |
| 80484 | T2 | 7.9% | 180 | 13.5% | — | 13.5% | Match, min $25,001 |
| 81434 | T2 | 12.9% | 180 | 1.5% | — | 1.5% | Match, min $25,001 |
