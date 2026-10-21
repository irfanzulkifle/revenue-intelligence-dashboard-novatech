# NovaTech Data Verification Log

**Student Name:** Muhammad Irfan bin Zulkifle
**Date:** 12 September 2026

## Instructions

Query each pre-indexed knowledge base using Quick Chat. For each question, record the expected answer (from the data dictionary), Q's actual response, and whether they match. Minimum 6 entries (2 per data knowledge base).

> **Note on method.** The pre-indexed knowledge bases described in the Environment Setup page
> (CRM Deals, Marketing Campaigns, Support Tickets, Company Background) were not present in the
> provisioned Vocareum workspace — the **Knowledge Bases** tab was empty under an "Owner: All"
> filter (screenshot attached). Verification was performed instead against the three
> instructor-provided `novatech_*.csv` datasets, queried through Quick Chat in **Specific data**
> mode with those three datasets attached. All questions, expected answers and pass/fail
> criteria are unchanged. Entries 10 and 11 were adapted from the Reference Documents knowledge
> base into cross-dataset integrity checks.
>
> Questions were phrased to name the dataset and column explicitly (for example, *"In the
> novatech_crm_deals dataset, how many rows are there?"*) because Specific data mode blends the
> model's general knowledge with the attached data. Naming the source forces grounding, and
> every response below carried a source citation.

## Verification Log

| # | Knowledge Base | Question Asked | Expected Answer | Q's Actual Answer | Match? | Notes |
|---|----------------|---------------|-----------------|-------------------|--------|-------|
| 1 | NovaTech CRM Deals | In the novatech_crm_deals dataset, how many rows are there? | 499 | "contains **499 rows** in total" | ✅ PASS | Cited source. |
| 2 | NovaTech CRM Deals | How many deals have deal_stage of Won and how many have Lost? | Won 315, Lost 184 (63.1%) | Won 315, Lost 184, "win rate of approximately **63.1%**" | ✅ PASS | Returned a table plus the derived win rate unprompted. Confirms `deal_stage` holds only two values — every row is a closed deal, so the dashboard reports win rate, not pipeline coverage. |
| 3 | NovaTech CRM Deals | How many rows have a blank or missing loss_reason? | 315 — exactly the Won count | "**315 rows** … aligns with the 315 deals that have a Won outcome" | ✅ PASS | Q correctly identified this as a structural null rather than missing data. Loss-reason analysis must filter to Lost deals or the 315 blanks dominate. |
| 4 | NovaTech Marketing Campaigns | How many rows are there, and what are the earliest and latest campaign_date? | 2,240; 2023-01-01 to 2025-01-31 | 2,240 rows; January 1 2023 to January 31 2025 | ✅ PASS | Marketing data begins ~5.5 months before CRM (2023-06-17). Cross-domain trend visuals must be filtered to the overlapping window or the early months read as zero sales. |
| 5 | NovaTech Marketing Campaigns | How many rows have a missing annual_income value? | 24 (1.1%) | "**24 rows** with a missing annual_income" | ✅ PASS | The one null count the dictionary states accurately. |
| 6 | NovaTech Marketing Campaigns | What percentage of rows have campaign_response equal to 1? | 27.2% — 609 of 2,240 | "**27.19%** … 609 out of 2,240" | ✅ PASS | Computed a derived rate correctly rather than retrieving a stored value. |
| 7 | NovaTech Support Tickets | How many rows are there, and how many rows in each priority level? | 3,000 — low 1,500, medium 1,050, high 400, critical 50 | 3,000 total; Low 1,500 (50%), Medium 1,050 (35%), High 400 (13.3%), Critical 50 (1.7%) | ✅ PASS | Counts exact. **Note:** Q returned title case (Low/Medium/High/Critical); the source values are lowercase. Q is normalising for display, so filter values in the dashboard must use the raw lowercase strings. |
| 8 | NovaTech Support Tickets | How many rows have a blank or missing customer_sentiment? | **Dictionary says 0. Actual data: 59 (2.0%).** | "**59 rows** … approximately 1.97% of the 3,000 total rows" | ✅ PASS on the count — ❌ **FAIL, documentation defect** | Q read the column correctly and found what the dictionary denies. **The data dictionary is wrong, not the data.** See Discrepancy 1 below — Q also made an unprompted false claim about these rows. |
| 9 | NovaTech Support Tickets | What is the average time between ticket_created_date and ticket_resolved_date, and how many rows have no ticket_resolved_date? | **58.70 hours** at full precision; **58.20 hours** as the dashboard calculates it (see note below); 59 unresolved | "approximately **1.95 days**" (= 46.8 hours); 59 rows with no resolved date | ⚠️ **PARTIAL FAIL** | Unresolved count correct. Q's average is **20% below even the lower of the two valid figures** — see Discrepancy 2. |
| 10 | Cross-dataset (adapted from Reference Documents) | How many distinct account_id values are in each dataset, and are any marketing or support accounts missing from CRM? | CRM 85, Marketing 100, Support 100; 15 orphans ACCT-101–115 | CRM 85, Marketing 100, Support 100; "**15 account_ids** … ACCT-101 through ACCT-115" | ✅ PASS | Took 4 steps — the only multi-step query. Q additionally observed that ACCT-086 to ACCT-100 appear in no dataset, which is **correct** (independently verified). **The check that determines the join design:** a CRM-anchored LEFT join excludes 354 rows by design. |
| 11 | Cross-dataset (adapted from Reference Documents) | How many distinct opportunity_id values compared to total rows, and how many distinct ticket_id values compared to total rows? | **Dictionary says both unique. Actual: 496 of 499, 2,996 of 3,000.** | CRM 499 rows / **496 distinct** ("3 rows share a duplicate"); Support 3,000 rows / **2,996 distinct** ("4 rows have duplicate ticket_ids") | ✅ PASS on the counts — ❌ **FAIL, documentation defect** | Q found both collisions and flagged them as a possible quality issue. **The dictionary documents both fields as "Unique identifier"; neither is.** See Discrepancy 3. |

**Result: 9 clean passes, 1 partial fail, and 3 defects surfaced.** Coverage is 11 entries against
a 6-entry minimum — 3 per data source plus 2 cross-dataset checks — spanning row counts, distinct
values, date ranges, null counts, categorical distributions, derived rates and referential integrity.

---

## Import verification — row and column counts

Each dataset was checked against the original CSV schema after import to SPICE. Screenshots of the
dataset Summary page (column count) and the Refresh history (rows ingested, rows skipped) are included
for all four.

| Dataset | CSV columns | Excluded | Calculated added | Columns in SPICE | Rows ingested | Rows skipped |
|---|---|---|---|---|---|---|
| `novatech_crm_deals.csv` | 20 | 1 | 4 | **23** | **499** | 0 |
| `novatech_marketing_campaigns.csv` | 20 | 2 | 4 | **22** | **2,240** | 0 |
| `novatech_support_tickets.csv` | 20 | 1 | 4 | **23** | **3,000** | 0 |
| `novatech_unified_revenue` (joined) | — | — | — | **68** | **63,420** | 0 |

**Every row count matches the data dictionary exactly, and no rows were skipped in any import** — so
no value failed a data-type conversion.

**Column counts differ from the 20 in each source file, by design.** The reconciliation:

**CRM — 20 → 23.** Excluded `data_source` (a constant, "CRM" on every row, so it can segment nothing).
Added `days_to_close`, `is_won`, `discount_pct`, `deal_key`.

**Marketing — 20 → 22.** Excluded `data_source` and `household_size` (household size of a lead contact
is a consumer attribute with no meaning in a business-to-business pipeline). Added `campaign_roi`,
`is_closed_won`, `is_response`, `funnel_stage_order`.

**Support — 20 → 23.** Excluded `data_source`. Added `resolution_hours`, `is_unresolved`,
`is_negative`, `ticket_key`.

**Unified — 68.** The sum of the three prepared datasets (23 + 22 + 23). No columns were dropped in the
join, and `account_id` appears three times — once per source — auto-renamed by Quick to `account_id`,
`account_id.1` and `account_id.2`. The join was built on `account_id.1`, the anchor table's key.

## Resolution-time calculation — two valid figures

Two different averages appear for the same field, and both are correct under their own definition:

| Calculation | Average | Where it appears |
|---|---|---|
| Full timestamp difference, fractional hours retained | **58.70 hrs** | Source-file computation |
| `dateDiff(..., "HH")`, which truncates the partial hour | **58.20 hrs** | The dashboard |

The 0.50-hour gap is systematic, not an error: `dateDiff` with an hour interval discards the remaining
minutes on every ticket, so each value is floored and the mean shifts down by roughly half an hour.

**The dashboard's figures are the ones quoted throughout this project**, because they are what a reader
can verify on screen. By priority:

| Priority | Full precision | Dashboard (`dateDiff`) |
|---|---|---|
| critical | 56.60 | **56.1** |
| medium | 58.00 | **57.5** |
| high | 58.12 | **57.6** |
| low | 59.42 | **58.9** |

The finding is unchanged by the choice: the spread between critical and low is **2.82 hours** at full
precision and **2.80 hours** as the dashboard calculates it. Hours rather than days was chosen
deliberately — see Discrepancy 2, where calculating in days is exactly what caused Q's error.

---

## Discrepancies found

### Discrepancy 1 — Q asserted a relationship that does not exist *(AI inference error)*

On Entry 9, Q volunteered, unprompted:

> *"Notably, the 59 tickets missing a resolved date matches the same 59 rows with missing
> customer_sentiment, suggesting these open tickets haven't had their sentiment recorded yet."*

**This is false.** Verified directly against the source file:

| Check | Rows |
|---|---|
| Missing `customer_sentiment` | 59 |
| Missing `ticket_resolved_date` | 59 |
| **Missing both (overlap)** | **0** |
| Missing sentiment but *has* a resolved date | 59 |
| Missing resolved date but *has* a sentiment | 59 |
| **Distinct rows carrying at least one gap** | **118** |

The two sets are completely disjoint. Q saw two counts of 59, inferred they were the same rows,
and offered a plausible causal story for it. Every sentiment-null ticket *was* resolved; every
unresolved ticket *does* have a sentiment recorded.

**Why it matters:** the inference is more dangerous than a wrong number, because it sounds like
insight and reads as a tidy explanation. Acting on it would mean assuming 59 rows are affected
when 118 are — and concluding the gap is a workflow artefact of open tickets when it is not.
**This is the clearest evidence in the project that AI output requires verification against the
data, not just against plausibility.**

### Discrepancy 2 — Average resolution time understated by 20% *(data type artefact)*

Q reported **1.95 days**, which is **46.8 hours**. The true average is **58.70 hours** at full
precision, or **58.20 hours** as the dashboard calculates it. Q is roughly 20% below either figure.

The cause is a data type mismatch in the source. `ticket_created_date` is a plain date
(`2024-07-27`) while `ticket_resolved_date` carries a timestamp (`2024-07-27 07:28:19`).
Subtracting calendar dates and discarding the time component gives exactly **1.946 days** — Q's
figure to three decimals. Q truncated to whole days rather than calculating across them.

**Mitigation applied in the build:** `ticket_resolved_date` was explicitly typed as a DateTime with
the `yyyy-MM-dd HH:mm:ss` mask, and resolution time is calculated in **hours** rather than days. Had
the field been left as a date, the dashboard would have reproduced Q's error. See the
resolution-time reconciliation above for the two valid figures and why the dashboard's is quoted.

### Discrepancy 3 — Two documented "unique identifiers" are not unique

`opportunity_id`: **496 distinct of 499** — OPP-44760, OPP-82512 and OPP-98039 each appear twice.
`ticket_id`: **2,996 distinct of 3,000** — TKT-284055, TKT-634299, TKT-679459, TKT-946331 each twice.

In every case the two rows are genuinely different records on different accounts, with different
dates, products and outcomes — ID-generation collisions, not duplicated records. There are **zero**
fully duplicated rows in any of the three files.

**Mitigation applied:** composite keys `opportunity_id + account_id` (499 unique) and
`ticket_id + account_id` (3,000 unique) replace the raw identifiers before any distinct count.
Without this, deal counts undercount by 3 and ticket counts by 4.

---

## Cross-Check

Pick one fact from above and confirm it independently in the QuickSight dataset preview.

- **Fact verified:** Total CRM deal count (Entry 1)
- **Chat said:** 499 rows
- **QuickSight shows:** 499 rows ingested, 0 skipped, on the `novatech_crm_deals.csv` **Refresh** tab
  (screenshot attached); dataset preview confirms 499 rows
- **Consistent?** ☑ **Yes**

**Second cross-check — the one that matters.** Entry 8 contradicts the documentation, so it was
confirmed independently:

- **Fact verified:** Null count in `customer_sentiment` (Entry 8)
- **Chat said:** 59 rows (1.97%)
- **Verified against the source file:** counting blank `customer_sentiment` values directly in
  `novatech_support_tickets.csv` returns **59 rows** of 3,000
- **Consistent?** ☑ **Yes — and the data dictionary is wrong, not the data**

**Third cross-check — disproving Q's inference (Discrepancy 1):**

- **Fact verified:** whether the sentiment-nulls and the resolved-date-nulls are the same rows
- **Chat said:** they are the same 59 rows
- **Verified against the source file:** rows where `customer_sentiment` is blank **AND**
  `ticket_resolved_date` is blank = **0**. The two sets are disjoint; 118 distinct rows carry at
  least one gap
- **Consistent?** ☑ **No — Q's claim is disproved**

**Note on method.** Checks two and three were performed against the source CSV rather than inside
Quick, because the claim being tested was produced *by* Quick. Confirming it in the same tool that
generated it would not be independent verification; going back to the raw file is.

---

## Conclusion

The indexed data is complete and internally consistent. Every row count, date boundary and
categorical distribution matched the dictionary exactly, and Q retrieved all of them correctly
with source citations.

The three defects break into two kinds, and the distinction is the point:

**Documentation defects (2).** The dictionary understates nulls in `customer_sentiment` and
claims uniqueness for two fields that have duplicates. Q read the data correctly in both cases —
the documentation is what is wrong. Both were mitigated in the preparation layer before any
visual was built.

**An AI reasoning defect (1).** Q asserted a relationship between two null sets that share no
rows at all, and wrapped it in a plausible explanation. No amount of data preparation prevents
this; only verification does. It is the single strongest argument in this project for keeping an
analyst between the natural-language layer and a business decision — and it surfaced on question
nine of eleven, in an answer that was otherwise correct.

A third issue, the 20% understated resolution time, is neither: it is a **data type artefact**
that directly shaped a build decision (hours rather than days, with an explicit DateTime mask).
