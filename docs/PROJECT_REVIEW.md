# Audit — NovaTech Revenue Intelligence Dashboard (attempt 2, passed 2026-09-13)

Source: `NovaTech_Revenue_Intelligence_Dashboard_AfterAdjustment_Muhammad_Irfan.zip`
82 files / 8.3 MB — 73 PNG, 5 PDF, 4 MD. No source CSVs included.

## Verdict

Meets every rubric line. The reviewer note says the same thing in one sentence: "all five rubric areas
with logs, preparation screenshots, interaction evidence, dashboard exports, and a stakeholder report."
The analytical writing is well above the minimum — the failure taxonomy in the Q log (wrong denominator →
fan-out → invented definition → correct conclusion on fabricated evidence → correct figures with inverted
conclusion) is the part no rubric asks for.

## Rubric coverage

| Rubric line | Evidence | Status |
|---|---|---|
| Verification log ≥6 entries, 3 bases, 4 fields each | `02_verification_log_updated.md` — 11 entries, 3 per source + 2 cross-dataset, all four fields present | pass |
| Import screenshots with row/column counts | `1 - Preindex…` (11 png), `2 - Upload Import…` (8 png); import table reconciles 20→23 / 20→22 / 20→23 / 68 columns, 0 rows skipped | pass |
| Data type corrections | `3 - Data-type Dialogs…` (12 png), incl. the `ticket_resolved_date` DateTime mask that prevents the 20% error | pass |
| ≥2 calculated fields | 4 per dataset (`days_to_close`, `is_won`, `discount_pct`; `campaign_roi`, `is_closed_won`, `is_response`, `funnel_stage_order`; `resolution_hours`, `is_unresolved`, `is_negative`, keys) | pass |
| Unified dataset + join diagram + anchor/join named | `4 - Unified Dataset` (3 png); anchor = CRM, LEFT join on `account_id`, 63,420-row fan-out disclosed and neutralised | pass |
| 4 datasets in SPICE | import table, 0 skipped | pass |
| 3 sheets + KPI cards | Marketing Funnel (4 KPI), Sales Pipeline, Customer Health | pass |
| Customer Health uses unified dataset | "Account Risk: Deals vs. Support Burden" scatter, r = +0.59 | pass |
| ≥2 sheets with filters; 1-click on 2+ visuals; cross-sheet nav | `5`/`6` sheet screenshots incl. filter state; `8 - Published Dashboard` | pass (weakest line — see below) |
| Dashboard exported as PDF, 3 sheets | 3 per-sheet PDFs + `NovaTech_Dashboard_Annotated_AllSheets.pdf` (3 pages) | pass |
| Baseline 2–3 Qs → Topic → same Qs re-asked | `9 - Quick Chat Baseline` (4 png), `10 - Topic Creation…` (4 png), `11 - 3 re-tests…` (18 png) | pass |
| Topic configured, screenshots of setup | custom instructions + relationships + published version, all captured | pass |
| Q log ≥5 entries, 3 domains, ≥1 cross-dataset | 5 new questions spanning sales / support / marketing / cross-dataset (Q3), each with dashboard cross-check | pass |
| 3–5 annotations, finding + implication + action | 5 annotations, all three components each, `12 - Annotated Sheets` (3 png) | pass |
| Report 1–3 pages, terms defined, no SQL/formulas, AI-vs-dashboard agreement | 2 pages, definitions inline ("row multiplication", "KPI cards", "correlation"), no formulas | pass |

## What the reviewer flagged (four items, all confirmed)

1. **Action semantics** — "region selection also filters deal stage, and account navigation does not
   visibly carry account context." Real. This is the one line where the PDF export cannot prove the
   interaction works, so the defect is invisible in the deliverable itself.
2. **Reconcile report details with final evidence and qualify causal claims from descriptive averages.**
   Real, and I found the specific instance: the report says "Ten questions were put to Quick Chat" —
   the Q log contains **8 distinct questions** (3 baseline + 5 new). Counting interactions instead of
   questions gives 11. Ten is neither.
3. **Reconcile the tally of clean passes.** Real, and off by one. The log claims "9 clean passes, 1
   partial fail, 3 defects". Actual: entries 1–7 and 10 are clean (**8**), entries 8 and 11 are
   "PASS on the count — FAIL, documentation defect" (2), entry 9 is a partial fail (1). The same
   number appears differently again in the report: "Ten checks were run… the other eight matched
   exactly" against a log that holds 11 entries and 3 defects.
4. **Production version: join account-level aggregates to reduce fan-out.** Forward-looking advice,
   not a defect.

The causal-qualification point also lands on annotation 3 ("closing half the gap is worth roughly 17
additional wins") and annotation 5 ("a quarter of the revenue base has no early warning") — both read
as consequences of a descriptive comparison, not of a tested intervention.

## Independent observations not in the feedback

- Dashboard title reads **"Novateach Revenue Intelligence"** — typo for NovaTech, visible in the tab and
  in the published-dashboard screenshots.
- Screenshots carry the account name **UdacityQuicksightLab** in the app header. No email address, no AWS
  account ID, no Vocareum ID is legible. Safe to publish as-is.
- The zip is a curated deliverable pack, not a working directory: no CSVs, no `.pbix`-equivalent export.
  That removes the data-redistribution question entirely for a public repo.

## Best material for a portfolio narrative

Ranked by how hard it would be for someone else to produce:

1. **The five failure modes**, ordered by detectability. "Correct figures, inverted conclusion" — Q read a
   2.8-hour spread across a four-level severity scale as evidence priority *works* — is the argument the
   whole project exists to make.
2. **The self-contradiction between two entry points.** Auto-generated executive summary says the sales
   cycle "surged… signaling growing delays"; Quick Chat, asked directly, finds it shortening and flags
   right-censoring. Same data, same product, opposite conclusions, and the unsolicited one is wrong.
3. **The 143× fan-out**, disclosed as a number rather than hidden: 63,420 rows from 5,739 records.
4. **Two documentation defects** (59 undocumented sentiment nulls; two "unique" identifiers that collide)
   proven against the raw CSV rather than inside the tool that produced the claim.
5. **Detection cost as a design principle** — the reason the log is ordered the way it is.
