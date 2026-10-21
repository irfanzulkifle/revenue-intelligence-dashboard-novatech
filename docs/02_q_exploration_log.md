# NovaTech Q Exploration Log

**Student Name:** Muhammad Irfan bin Zulkifle
**Date:** 12 September 2026

---

# Part 1 — Before/After Topic Comparison (Tasks 9 & 11)

**Baseline method:** Quick Chat scoped to the **published dashboard only**, no Topic configured.
**Post-Topic method:** Quick Chat scoped to the **Topic only** (`NovaTech Revenue Intelligence`,
Version 2 Active). One variable changed between the two runs.

| # | Question | Verified correct answer | Baseline (dashboard only) | Post-Topic | Verdict |
|---|---------|------------------------|---------------------------|------------|---------|
| 1 | What is our win rate? | **63.1%** (315 of 499) | **63.1%** — correct, cited Sales Pipeline sheet and date range | **63.1%** — correct, cited "all 499 CRM deals" | No change. Slightly more precise sourcing. |
| 2 | Which campaign channel converts best? | **Direct Mail, 49.0%** (73 of 149) | **Correct.** Full ranking exact to 2dp: 48.99 / 31.10 / 22.77 / 2.78 / 0.89. Resolved "converts" to lead-to-closed-won unprompted | **Percentages correct, supporting table WRONG.** See Discrepancy A | **Regression** |
| 3 | Show me our at-risk accounts. | Requires ticket volume + negative sentiment + revenue combined | **Wrong list.** Invented a tickets-per-deal definition; only 3 of 10 accounts overlapped with the correct list; missed 40% of at-risk revenue | **Correct.** Four criteria, every figure exact, $0-revenue accounts correctly deprioritised | **Major improvement** |

**Headline finding: Topic configuration is not monotonic improvement.** It fixed the question that
required cross-domain reasoning and introduced an error on one that did not.

---

## Discrepancy A — Post-Topic table counts are wrong while percentages are right (Q2)

Q's post-Topic answer displayed a "Conversion Rate By Campaign Channel" table:

| Channel | Q's leads | True leads | Q's conversions | True conversions | Q's ratio | True ratio |
|---|---|---|---|---|---|---|
| Direct Mail | 149 ✓ | 149 | 73 ✓ | 73 | 48.99% | 48.99% |
| Partner Referral | **148** | **807** | **46** | **251** | 31.08% | 31.10% |
| Paid Social | **149** | **325** | **34** | **74** | 22.82% | 22.77% |
| Email | **143** | **288** | **4** | **8** | 2.80% | 2.78% |
| Organic Search | **111** | **671** | **1** | **6** | 0.90% | 0.89% |

**Q's table totals 700 leads. The dataset contains 2,240.** Only Direct Mail is exact.

The ratios are preserved to within 0.05 percentage points, so the narrative conclusion — Direct Mail
converts best — remains correct. But the counts it is derived from are wrong for four of five channels.

**Why this matters more than it first appears.** The *baseline* answer gave percentages alone, and
they were correct. The post-Topic answer shows its working, and the working is wrong. An analyst
who trusted the table would report that Partner Referral generated 148 leads when it generated 807 —
an error of 659 leads in the largest channel by volume. Adding a semantic layer made the answer
*look* more auditable while making one layer of it less accurate.

---

## Discrepancy B — Baseline at-risk list was materially wrong (Q3, pre-Topic)

Sarah Chen's brief defines at-risk as *"accounts that have high ticket volume, negative sentiment,
AND high deal value."* Pre-Topic, Q used **one** of those three — tickets per deal.

| | Q's baseline list | Correct list (Sarah's criteria) |
|---|---|---|
| Accounts overlapping | **3 of 10** — YieldMax, LionGate, RedCedar | **3 of 10** — same three |
| Revenue covered | **$110,466** | **$182,814** |

Omitted from Q's list: **Falcon Software** (40.5% negative sentiment, $20,912 revenue — second-highest
risk overall), **TrueNorth Electronics** (180 tickets, 25.6% negative, $16,747), AlphaCore, HavenCrest,
PeakVista, PrimeCell, StoneHarbor.

Wrongly included: **SapphireCode Software** and **KaleidoScope Retail**, both with **$0 won revenue** —
every deal lost. There is no revenue at risk, so they cannot be at-risk accounts in Sarah's sense.

**Q could not have done better pre-Topic.** The scatter plot it read carries no revenue and no
sentiment — currency was deliberately excluded from the unified dataset to avoid the 143× fan-out.
Q answered using the only two fields exposed to it, and its arithmetic was correct throughout
(MapleCrest 109 tickets / 4 deals = 27.2 ✓). The failure was in what the dashboard could expose,
not in the reasoning.

---

## Post-Topic Q3 in detail — what the Topic actually changed

Q expanded to **four** risk indicators: high ticket volume, negative customer sentiment, unresolved
tickets, and lost deals. It reported 97 accounts with at least one signal and 17 with all four.

Every figure verified against source:

| Account | Tickets | Negative | Unresolved | Lost deals | Won revenue |
|---|---|---|---|---|---|
| ACCT-041 | 334 ✓ | 63 ✓ | 7 ✓ | 7 ✓ | $40,722 ✓ |
| ACCT-035 | 180 ✓ | 46 ✓ | | | $16,747 |
| ACCT-076 | 176 ✓ | 38 ✓ | | | $16,275 |
| ACCT-043 | 171 ✓ | 36 ✓ | | | $14,874 |
| ACCT-060 | 142 ✓ | 35 ✓ | | | $15,780 |
| ACCT-036 | 140 ✓ | 37 ✓ | | | $7,950 |
| ACCT-020 | 109 ✓ | 22 ✓ | | | $6,006 |
| ACCT-072 | 90 ✓ | 28 | | | $13,789 |
| ACCT-011 | 37 | 15 | | | **$20,912 ✓** |
| ACCT-062 | 49 | 13 | | | **$17,362 ✓** |

**Three specific changes, each traceable to a configuration choice:**

1. **ACCT-011 and ACCT-062 now appear.** Both were absent pre-Topic. They rank #2 and #3 on Sarah's
   criteria. Attributable to the **dataset relationships** — Q can now reach CRM revenue from support
   data without the pre-joined dataset.

2. **ACCT-053 is correctly deprioritised.** Q noted it *"shows risk signals but has $0 in won revenue,
   suggesting it may already be a churned or pre-revenue account."* That is the custom instruction
   *"An account with zero closed-won revenue is not at risk, because there is no revenue to lose"*
   applied directly. Pre-Topic, two such accounts sat in the at-risk list unqualified.

3. **Revenue is quoted at all.** Pre-Topic it could not be, because no visual exposed it.

---

# Part 2 — Q Exploration Questions (Task 11)

Five new questions, asked in a single continuous session scoped to the **Topic only**
(`NovaTech Revenue Intelligence`, Version 2 Active). Domain coverage: Sales (1, 5), Support (2),
Marketing (4), cross-dataset (3).

**How verification was done, and the distinction being drawn.** Each answer was checked in two ways
where both were possible. **Dashboard verification** means the figure was read off a named visual at
the same level of aggregation Q used, with the filter state recorded — this is what the "Dashboard
Shows" column reports. **Source-file verification** means the figure was computed independently from
the original CSV, and is labelled as such in the Notes. The two are not interchangeable: a dashboard
check confirms Q and the dashboard agree; a source-file check confirms both are right. Where an
answer went beyond what any visual displays — a 30-combination breakdown, for instance — that portion
is marked source-file verified rather than presented as a dashboard match.

| # | Question Asked | Q's Answer (summarize) | Dashboard Visual Used to Cross-Check | Dashboard Shows | Match? | Notes |
|---|---------------|----------------------|--------------------------------------|-----------------|--------|-------|
| 1 | What is the average deal size by company size? | Enterprise $2,515 (127 won) · Small $2,230 (26) · Large $2,042 (133) · Medium $2,007 (29). Led with "(based on won deals)" | **Sales Pipeline — "Average Deal Size by Company Size Tier"** (visual-level filter `deal_stage = Won`; all sheet controls set to All) | **Enterprise $2,515 · Small $2,230 · Large $2,042 · Medium $2,007** — read directly from the dashboard visual | ✅ **Exact — verified on the dashboard** | **Topic effect.** Without the custom instruction this returns $1,589 / $1,486 / $1,353 / $1,257 by averaging in the 184 lost deals at $0. Q applied the won-deals filter unprompted and said so. *Note: this visual was added after the first submission — the dashboard originally had no by-company-size breakdown, so the earlier version of this log claimed a dashboard check that did not exist. The won-deal counts (127/26/133/29) remain source-file verified, as the visual shows averages only.* |
| 2 | What is the average resolution time for critical versus low-priority tickets? | Critical **56.1 hrs** (48 tickets) · Low **58.9 hrs** (1,465). Called the 2.8-hour gap "surprising" | Customer Health — Resolution Time by Priority | critical 56.1 · medium 57.5 · high 57.6 · low 58.9 | ✅ Figures exact / ❌ **interpretation inverted** | **See Discrepancy C.** Q concluded the small gap "suggests critical tickets may be getting prioritized appropriately." The correct reading is the opposite. |
| 3 | What are the top 10 accounts by support ticket volume, and what is their total deal revenue? | All ten accounts in correct rank order with revenue joined. ACCT-041 334 tickets / **$40,722**. Revenue range **$6,006–$40,722** | Customer Health — Top Accounts, cross-checked vs Sales Pipeline revenue | 334 / 180 / 176 / 171 / 142 / 140 / 109 / 90 / 89 / 83. **Totals: 1,514 tickets (50.5% of all support load) and $154,709 (21.9% of revenue)** | ✅ Data exact / ⚠️ **incomplete** | **Cross-dataset, and no fan-out** — it returned $40,722, not an inflated figure. The Topic relationships worked. But the question asked for the **total**; Q gave a *range* and never summed. The two percentages side by side are the finding, and Q left them on the table. |
| 4 | Are there any campaigns where we spent more than we earned back? | **All 30 campaign-channel combinations negative.** Top 10 by spend listed. Partner Referral: 6 combinations, >$7.2M spend, ~$677K returned. Best: NovaPulse Launch via Direct Mail at −51.0% | **Marketing Funnel, filtered `campaign_channel = Partner Referral`** (all other controls All) — KPI cards plus the "Campaign Spend vs. Attributed Revenue" chart | With that filter: **Total Campaign Spend $7,270,706** · Total Leads **807** · Lead-to-Won Conversion **31.1%** · funnel Qualified Lead 528 / Closed Won 251. Chart tooltip on the leading bar: **NovaPulse Launch, campaign_spend 1,620,452**. Six campaign bars in the order NovaPulse Launch → Digital Retarget → Enterprise Expansion → Year-End Accelerator → NovaEdge Awareness → Q3 Growth Sprint | ✅ **Exact — verified on the dashboard at channel granularity** | **Strongest Topic effect in the project.** Q went more granular than asked (30 pairs, not 6 campaigns) and closed with its own heading, "A Note on Interpretation": *"Campaign spend is attributed to all touchpoints (every lead), while revenue is only credited to converters… ROI figures reflect relative efficiency rather than absolute profitability."* That is the custom instruction reproduced as reasoning, unprompted. *Source-file verified separately (not shown on any dashboard visual): the remaining five per-campaign spend figures, the full 30-combination negative result, the −51.0% best ROI, and the two combinations at exactly $0 revenue.* |
| 5 | Is our sales cycle getting longer or shorter? | Shortening. Grouped by **deal creation month**: 172 days (Jun 2023) → 83 (Dec 2023) → 42 (Feb 2024) → 36 (Jun 2024). Flagged survivorship bias in recent cohorts and restricted its conclusion to the complete-data window | Sales Pipeline — Sales Cycle Length Over Time | Dashboard groups by **close date, by quarter**: 79.5 → 51.5 → 64.7 → 85.6 → 54.4 → 90.1 — no trend | ✅ Exact / **Q's framing is better than the dashboard's** | **See Discrepancy D.** Q chose creation cohorts over close-date buckets, which is the correct unit for cycle-length trend, and identified right-censoring in the data unprompted. |

---

## Discrepancy C — Correct numbers, inverted conclusion (Q2)

Q reported critical at 56.1 hours and low at 58.9, then wrote:

> *"The small gap suggests that critical tickets may be getting prioritized appropriately."*

**This reverses the finding.** A 2.8-hour spread across a four-level severity scale means priority is
**not** driving triage. A customer reporting a system outage waits about as long as one asking a
routine question. The flatness is the problem, not evidence of a working process.

A secondary overstatement in the same answer: *"Low-priority tickets make up the vast majority of
volume (1,465 vs. 48)"* — true only against critical. Low is 48.8% of all 3,000 tickets, and medium
accounts for a further 1,037.

**Why this is the most dangerous failure mode observed.** A wrong number can be caught by anyone who
knows the data. Fabricated evidence can be caught with ground truth. An inverted conclusion sits on
top of correct figures and a correct chart, and can only be caught by someone who already understands
what the result means. An executive skimming this answer closes the tab reassured, and the cheapest
improvement available in the dataset — a routing rule — never gets made.

## Discrepancy D — Q's analytical framing was better than the dashboard's (Q5)

Asked about the sales cycle trend, the dashboard visual groups `days_to_close` by **close date, by
quarter**, producing 79.5 → 51.5 → 64.7 → 85.6 → 54.4 → 90.1: no discernible trend.

Q grouped by **deal creation month** instead, producing a clear decline from 172 days to roughly 36
over the same period.

**Q's choice is the correct one.** Close-date buckets mix deals originating in many different periods,
so a quarter's average describes whatever happened to close in it rather than how long deals take.
Creation cohorts answer the question asked.

Q then identified the flaw in its own output:

> *"The sharp drop in the most recent months (Oct 2024 – Jan 2025) is likely survivorship bias — only
> the fastest-closing deals from those cohorts have closed so far… The true average for recent months
> will likely drift upward as more deals close."*

Verified and correct: those cohorts show 18.5, 10.4, 20.5 and 7.5 days because the data ends
31 January 2025. Sixty-two deals were created in that window and 32 have closed. Q recognised
right-censoring in time-to-event data unprompted, and correctly restricted its conclusion to
*"mid-2023 through mid-2024 — where we have complete data."*

**Recommended dashboard change:** rebuild the Sales Cycle Length visual on `deal_created_date` rather
than `deal_closed_date`, and cut the trailing months where cohorts are incomplete.

---

## Reflection (include in written summary)

**Where did Q agree with the dashboard?**

On every question whose answer sits on a single, well-titled visual. Win rate, resolution time by
priority, top accounts by ticket volume, and the campaign ROI ranking all matched exactly — pre-Topic
as well as post-Topic. Notably, the *baseline* already performed well: scoped to the published
dashboard, Q answered two of three baseline questions correctly before any Topic existed, because the
KPI cards and axis labels were written in business language. The dashboard's titles were already doing
part of a semantic layer's job.

**Where did Q disagree or struggle? Why?**

Five distinct failure modes, in increasing order of how hard each is to detect:

1. **Wrong denominator.** Averaging deal value across all 499 deals rather than the 315 won gives
   $1,417 instead of $2,245. Caught by anyone who knows the data — and prevented entirely once the
   custom instruction was in place.

2. **Fan-out inflation.** Revenue questions against the joined dataset can return figures up to 143×
   too high. Caught because the number is absurd.

3. **Invented definitions.** Pre-Topic, asked for at-risk accounts, Q chose one of Sarah's three
   criteria and produced a list overlapping the correct one by 3 of 10, missing 40% of the revenue at
   risk. Not caught without knowing the right answer.

4. **Correct conclusions on fabricated evidence.** Post-Topic, Q's channel table showed 700 leads
   where there are 2,240, with four of five counts wrong — while the percentages derived from them
   were right to two decimal places. Not caught without independent ground truth.

5. **Correct figures, inverted conclusion.** Q read a near-flat resolution-time result as evidence
   that critical tickets are "prioritized appropriately." The numbers, the chart and the arithmetic
   are all correct; only the meaning is wrong. **Not caught by anyone who does not already know what
   the result implies.**

The last two are the ones worth an executive's attention. The first three announce themselves. Modes
4 and 5 look exactly like good analysis: a conclusion, a table of workings, a confident sentence.

**Where did Q outperform the dashboard?**

Twice, and both are worth recording because a log that only catalogues AI errors is not an honest
evaluation.

On **campaign ROI**, Q volunteered the attribution caveat itself, under a heading it wrote called
"A Note on Interpretation" — explaining that spend is charged to every touchpoint while revenue is
credited only to converters, and that ROI therefore measures relative efficiency rather than
profitability. In the pre-Topic draft of this log, that question was the flagship example of AI being
technically right and commercially misleading. After configuration, Q warns the reader itself.

On the **sales cycle**, Q chose a better analytical framing than the dashboard does. The dashboard
groups by close date and shows no trend; Q grouped by creation cohort, found a clear decline, and then
identified right-censoring in its own recent data before drawing a conclusion. That is a judgement
call the dashboard's design does not make.

**When would you use Q vs. the dashboard to answer a business question?**

Use the **dashboard** for the recurring weekly questions — the metrics it was built around, where
definitions are settled and the numbers reconcile against each other by construction.

Use **Q** for the ad-hoc question nobody anticipated, where building a visual would cost more than the
answer is worth.

The boundary is not difficulty — it is **verifiability**. Q is safe when its answer can be checked
against something already on the dashboard within seconds. It is unsafe when it produces a number that
appears nowhere else, which is precisely when it is most tempting to trust. The at-risk question is the
clean example: it is the most valuable question in the brief, the one Q handles best after
configuration, and the one with no dashboard figure to check it against.
