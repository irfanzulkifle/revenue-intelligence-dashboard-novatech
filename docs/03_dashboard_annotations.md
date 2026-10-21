# Dashboard Annotations
**Project:** NovaTech Revenue Intelligence Dashboard  
**Analyst:** Muhammad Irfan bin Zulkifle

Five annotations, one per dashboard area. Each states a quantified finding, why it matters to
the business, and a specific recommended action — the three things the rubric checks.

**How to add them in Amazon Quick:** in your analysis, **Insert → Add text box**, place it
beside the visual named, paste the text, then republish the dashboard before taking your final
screenshots. Keep the text box narrow enough that it doesn't cover the chart.

**📸 After adding all five, screenshot each sheet with its annotations visible.**

---

## Annotation 1 — Marketing Funnel sheet
**Place beside:** "Cost per Closed-Won Lead by Channel" chart

> **Direct Mail converts 49% of leads to closed-won business at $9,881 per win. Organic Search
> converts 0.9% at $256,709 per win — 26 times more expensive.**
>
> Organic Search absorbs 671 leads, the second-largest volume of any channel, and returns six
> closed deals. Direct Mail handles a fifth of that volume and returns 73.
>
> **Action:** shift budget from Organic Search into Direct Mail and Partner Referral for the
> next two quarters, and set a floor of 20% closed-won conversion for any channel to keep its
> funding. Before cutting Organic Search entirely, confirm with Marketing whether it is
> intended as a top-of-funnel awareness channel scored on a different metric — its 294
> Prospect-stage leads suggest it may be.

---

## Annotation 2 — Marketing Funnel sheet
**Place beside:** "Campaign Spend vs. Attributed Revenue" combo chart

> **Reported campaign ROI is negative for all six campaigns, from −83.7% to −97.7%. Treat these
> as a ranking, not as profitability.**
>
> The marketing platform charges a fully-loaded cost to every one of the 2,240 touchpoints
> ($12.4M total) but credits revenue only to the 609 that converted ($1.13M). Against that,
> the CRM records $707,201 of actual closed-won revenue for the same period — so the two
> systems disagree about marketing's contribution by a factor of 1.6.
>
> **Action:** use the ranking to allocate — NovaPulse Launch is the strongest campaign on every
> measure (33.7% conversion, $963 revenue per lead, best ROI) and NovaEdge Awareness the
> weakest (5.1%, $126, worst ROI). Then commission Marketing Ops to reconcile the attribution
> model against CRM closed-won revenue before ROI is quoted in any board material.

---

## Annotation 3 — Sales Pipeline sheet
**Place beside:** "Win Rate by Sales Manager" bar chart

> **Win rates range from 70.8% to 50.0% across the six sales managers — a 20.8-point spread on
> comparable deal volumes.**
>
> David Kim closes 80 of 113 deals (70.8%, $174,158). Brian Nakamura closes 46 of 92 (50.0%,
> $103,168) on a similar book. The gap is not explained by deal size: both teams average close
> to the company's $2,245 won-deal value. The same pattern shows by region, where Central runs
> 69.9% against East at 57.8%.
>
> **Action:** run a win/loss review across Brian Nakamura's and Michael Chen's teams (51.4%)
> against David Kim's, focused on the two largest loss reasons — No Decision Made (43 deals)
> and Poor Product Fit (43). Together those are 47% of all losses and both are qualification
> problems, not pricing ones. Closing half the gap on those two teams is worth roughly 17
> additional wins over the 19 months measured — about 10 a year.

---

## Annotation 4 — Customer Health sheet
**Place beside:** "Resolution Time by Priority" bar chart

> **Critical tickets are resolved in 56.1 hours on average. Low-priority tickets take 58.9 hours.
> Priority is being recorded but is not driving triage.**
>
> A 2.8-hour difference across a four-level severity scale means a customer reporting a system-down
> incident waits essentially as long as one asking a routine question. Critical and high-priority
> tickets also carry the worst sentiment of any severity level — 28.0% and 28.7% negative, against
> 20.6% for low. Forty-one tickets in the dataset affected payment processing.
>
> **Action:** introduce priority-based SLA targets — a suggested 8 hours for critical and 24 for
> high — and route critical tickets to a dedicated queue rather than the shared backlog. This is the
> cheapest change in the dashboard: a routing rule, not headcount, addressing the 50 tickets most
> likely to trigger a churn conversation.
>
> *This finding is easy to read backwards.* Asked to compare these same two numbers, Quick Chat
> concluded the small gap "suggests critical tickets may be getting prioritized appropriately." The
> figures it used were correct; the interpretation was the reverse of the truth. A near-flat result
> across a severity scale is evidence the scale is not working, not evidence that it is.

---

## Annotation 5 — Customer Health sheet
**Place beside:** "Account Risk: Deals vs. Support Burden" scatter plot (unified dataset)

> **Our biggest customers are our biggest support burden. Account revenue and ticket volume
> correlate at +0.59, and the ten highest-risk accounts hold $182,814 — 25.9% of all
> closed-won revenue.**
>
> YieldMax Software (ACCT-041) is the clearest case: 334 tickets, 11.1% of everything the
> support team handles, against $40,722 of revenue, 5.8% of the company total. Falcon Software
> (ACCT-011) files fewer tickets but 40.5% of them carry negative sentiment, on $20,912 of
> revenue. Six accounts sit above 100 tickets each.
>
> **Action:** stand up a named-account success programme for the top ten by combined revenue
> and ticket load, with a quarterly business review and a named support contact. A quarter of
> the revenue base currently has no early-warning mechanism at all — this dashboard is the
> first place these accounts have ever appeared together.
>
> One caveat before acting: SapphireCode Software (ACCT-083) files 80 tickets at 31.3% negative
> sentiment but has no closed-won revenue at all — all four of its deals were lost. There is
> nothing to lose there, which is why it is not on the priority list.

---

## Coverage check

| Annotation | Sheet | Quantified finding | Business implication | Recommended action |
|---|---|---|---|---|
| 1 | Marketing Funnel | 49% vs 0.9% conversion; $9,881 vs $256,709 per win | 671 leads returning 6 deals | Shift budget; 20% conversion floor |
| 2 | Marketing Funnel | ROI −83.7% to −97.7%; $12.4M vs $1.13M | Attribution disagrees with CRM 1.6× | Rank not profitability; reconcile model |
| 3 | Sales Pipeline | 70.8% vs 50.0% win rate, 20.8pt spread | ~17 wins over 19 months at stake | Win/loss review on qualification |
| 4 | Customer Health | 56.1 vs 58.9 hrs across severity | Severity scale is non-functional | Priority SLAs + dedicated queue |
| 5 | Customer Health | r = +0.59; $182,814 = 25.9% of revenue | Quarter of revenue has no early warning | Named-account success programme |

Five annotations against a 3–5 requirement, spread across all three sheets.
