# Upsell Compass – MERP prototype

Open `index.html` in Chrome or Edge (double-click). No server, build or backend needed.
Desktop: phone frame + Dev/Testing panel side by side. Mobile: tap the "Test panel" tab on the left edge.

## Journey
Seller Profile → Upsell Compass + "Why this recommendation?" → Proposals → **+** → Recommended Proposal →
Edit Proposal → Update & Preview → Email Preview → Send (simulated) → Confirmation → history card with **View Email**.

## Scenarios (Dev panel → Scenarios)
| ID | Seller | What it shows |
|---|---|---|
| 1.1 | ABCD Enterprises | IM Star Pro (R2), 2 non-opted A-rank MCATs |
| 1.2 | ABCD Enterprises | A-rank insufficient → B-rank fallback |
| 1.3 | ABCD Enterprises | No MCAT qualifies → recommendation held, send blocked until executive adds one |
| 1.4 | ABCD Enterprises (districts 18) | Preferred Star Pro (R1), BL > 5 criterion |
| 2.1 | Sharma Industries | BL DAU 12 → Focus on Servicing → India Star MYR |
| 2.2 | Sharma Industries (on MYR) | Focus on Servicing, no duplicate MYR proposal |
| 3.1 | Metro Engineering | PNS 55% → India Star MYR |
| 4.1 | Global Pumps | Total A Rank 0 → India Star MYR |
| 5.1 | Prime Industrial | Preferred Star Pro (R3), 2 opted A + 1 B fill |
| 5.2 | Prime Industrial | Only 1 of 3 MCATs qualifies, proposal still generated |
| 5.3 | Prime Industrial (districts 25) | IM Star Pro (R4) |

Scenarios 1.4, 2.2 and 5.3 change one metric of the original seller so all four service rules and the MYR-holder case have a live example.

## Where things live
| File | Purpose |
|---|---|
| `js/config.js` | Thresholds, rule tables, MCAT criteria + selection order, prices, offers (also editable live in Dev panel → Config) |
| `js/data.js` | Dummy sellers, MCATs (rank, opted, DIR rank, 30-day BL, price tier), scenarios, existing proposals |
| `js/engine/recommendation-engine.js` | Servicing checks (OR) and upsell rule table |
| `js/engine/mcat-engine.js` | MCAT eligibility + configurable selection order |
| `js/engine/pricing-engine.js` | MRP / Deal Value / discount / GST / TDS / part payment |
| `js/engine/proposal-service.js` | Proposal generation, edits, simulated send, tracking record |
| `js/email/email-templates.js` | Separate content modules: India Star MYR renewal, IM Star Pro, Preferred Star Pro, generic |
| `js/ui/*` | Screens (SellerProfile, Proposals/RecommendedProposalCard/History, ProposalEditor, EmailPreview, DevPanel) |
| `js/tests.js` | Section 13 acceptance tests (Dev panel → Tests) |

## Assumptions to confirm with business
- **Pricing is per MCAT.** IM Star Pro / Preferred Star Pro use per-category Standard / Premium / Platinum prices (from the Differential Price sheet). Each MCAT carries a tier in the dummy data, and the proposal total is the sum of its MCATs, as in the current MERP flow.
- **MYR price is only known for Standard** (₹1,65,000 / ₹3,30,000 from the Sale Price sheet). Premium and Platinum MYR are left `null`, and the editor blocks sending until they are configured.
- The card **Amount includes 18% GST**, the same as existing MERP cards (₹4,00,000 → ₹4,72,000).
- **MCAT tie-break:** sort by DIR rank (ascending), then 30-day BL (descending). Step S4 (A-rank MCATs outside the scenario's opted / non-opted preference) is **off** until business confirms it.
- TDS defaults to 2%, and the "10% cashback" offer is a dummy that does not reduce Deal Value. The executive name and contact details are dummies.
- State is saved in the browser's localStorage for convenience. **Reset prototype** in the Dev panel clears it.
