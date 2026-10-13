---
name: tableau
description: Answers ad hoc business-metric questions from the company's Tableau Cloud data (signups, conversions, revenue, chargebacks, super partner and vertical performance, marketing channels, split tests and other KPIs) using standardized metric definitions, the right datasource for each department, and as few Tableau calls as possible. Use this skill whenever someone asks for a number, trend, comparison or breakdown of a company KPI ("how did paid social signups do last month?", "chargeback rate by super partner", "MoM revenue"), mentions a Tableau dashboard, view or datasource
---

<!-- TODO(BI): add the real KPI names and team names people use to the description above; it decides when the skill activates. -->
<!-- Owner: TODO · Last reviewed: YYYY-MM-DD -->

# Tableau metrics assistant

Answer business questions with numbers that match our Tableau dashboards, using the fewest Tableau calls possible.

## Why this skill exists

- **Correct, consistent numbers.** Every metric has one standard name and one definition (`references/glossary.md` plus the datasource cards). Answers should match what the dashboards show.
- **Tableau calls are limited and paid for.** The Tableau query service has a site-wide cap (100 queries per hour per Creator license) shared by everyone, and the company pays for usage. Everything needed to build a query is written in this skill, so a typical question needs **0 discovery calls and 1 query**.
- **Traceable answers.** Every answer ends with a receipt so the BI team can check it at a glance.

## Workflow

Follow these steps in order. Steps 1–4 and 6–7 use only the files in this skill, with no Tableau calls, unless the data isn't in the routing table (step 4).

1. **Identify the department.** Use the routing table below. Route by the question's topic first; use the user's own team only as a tie-breaker. If it's still unclear, ask once which department's data they need, offering the departments in the routing table as options. 
2. **Understand the question.** Map the user's words to standard metric names with `references/glossary.md`, then apply any department vocabulary from the department file.
3. **Clarify or assume.** See "Ask vs. assume" below. Ask at most 3 short questions, and only for things that change the answer. Offer valid values from the datasource card as options.
4. **Route to data.**
   - By default, use the datasource card. Read only the card(s) you need.
   - Use an approved view from the department file (`get-view-data` with `viewFilters`) only when it answers the exact question (same metric, breakdown and filters), or when the user names a dashboard.
   - If the data isn't in the routing table, search for it before saying it can't be found (see "Tableau call rules").
5. **Build and run one query.** Check `references/known-issues.md` and `references/query-patterns.md` first. Put every measure, dimension and period into a single aggregated query.
6. **Compute and check.** Calculate MoM changes, shares and ratios yourself from the result (ratios = SUM/SUM; never average a ratio). Run the checks in `references/qa-template.md`.
7. **Answer.** Use the output template below.
8. **Follow-ups.** Reuse data already in the conversation. Query again only for a new metric, dimension or period.
9. **Report issues to the BI team (any step).** When the user asks, or when a rule below says to, prepare a report email. See "Reporting issues to the BI team".

If you can't get a reliable answer at any step, follow `references/fail-safe.md`.

## Routing table

<!-- INCOMPLETE Datasource cards listed here are planned paths; the cards aren't written yet. -->

| Department | Teams / aliases | Typical topics | Tableau folder | Default traffic type | Department file | Datasource cards |
|---|---|---|---|---|---|---|
| Internal Marketing | SEM, paid social, facebook, tiktok | SEM performance by brand, channel (paid search, display) and engine (Google, Bing); SEM intraday monitor; paid social campaign and ad set performance; paid social period comparison; remarketing (email/SMS, per the folder description) | `Internal Marketing` (subfolders `SEM`, `Paid Social`) | SEM: super partner `110 Internal_SEM`. Paid Social: traffic_type = `Paid` and Platform = `Facebook` | `references/departments/internal-marketing.md` | `references/datasources/daily-core-business-overview.md`; `references/datasources/intraday-hourly-conversion.md` (SEM intraday); `references/datasources/facebook-adset-performance.md` (paid social) |
| External Marketing | external | super partner performance and deep dive; partner ID and affiliate ID breakdown; lead post success; weekly report | `External Marketing` | `IS_EXTERNAL_MARKETING` = True (super partner ID > 200) | `references/departments/external-marketing.md` | `references/datasources/daily-core-business-overview.md` |
| Advertising | adv, remarketing | advertising performance (folder description); TODO | `Advertising` (no workbooks visible yet) | TODO | `references/departments/advertising.md` | TODO |
| Finance | TODO | financial metrics, revenue tracking, operational cost analysis (folder description); TODO | `Finance` (no workbooks visible yet) | TODO | `references/departments/finance.md` | TODO |
| Billing | TODO | chargebacks; TODO | `Billing` (no workbooks visible yet) | TODO | `references/departments/billing.md` | TODO |
| Customer Service | CO, CSR, TSR | CSR/TSR performance, ticket resolution efficiency, customer cancels (folder description) | `Customer Service` (CSR and TSR subfolders; no workbooks visible yet) | TODO | `references/departments/customer-service.md` | TODO |
| Split Test | ab testing | A/B test results by test ID (e.g., T1367): variant comparison, test breakdown, user journey | `Split Test` | None: each test dashboard covers one test | `references/departments/split-test.md` | `references/datasources/split-test-core.md`; `references/datasources/split-test-user-journey.md` |
| Company Overview | TODO | company-wide KPIs; daily RE and Credco KPIs; company intraday monitor; company data explorer; user action tracking | `Company Overview` | All traffic | `references/departments/company-overview.md` | `references/datasources/daily-core-business-overview.md`; `references/datasources/intraday-hourly-conversion.md`; `references/datasources/user-action-tracking.md` |
| Executive Team | CEO, executives, exec team, leadership, C-suite | Usually company-wide KPIs, so use Company Overview; if the question is about a specific department's topic, route by topic instead | `Company Overview` | All traffic (same as Company Overview) | `references/departments/company-overview.md` | `references/datasources/daily-core-business-overview.md`; `references/datasources/intraday-hourly-conversion.md`; `references/datasources/user-action-tracking.md` |

Datasource cards live at `references/datasources/<datasource-name>.md`. List each card's full path in the table above so it can be opened directly.

**Default traffic type** is the traffic-type filter the department's dashboards apply by default. Use it unless the user asks for something else, and state it in the receipt.

**Where datasources live:** published datasources are in the `Data Sources` folder (certified ones are marked). TODO(BI): `Data Sources/Internal Data Sources` holds building-block tables (e.g., `DIM_CALENDAR`, `AGG_ORDERS`); confirm whether Claude should ever query them directly.

**Ignored Tableau folders** (never use as a source): Archive, Development, default, including their subfolders (e.g., `Development/Split Test`, `Development/Product Analysis`, `Samples`, `Admin Insights`); External Assets Default Project

## Ask vs. assume

Asking costs the user a moment; a wrong filter wastes a query and gives a wrong number. Ask only when the missing piece changes the answer; otherwise use the default and say so in the receipt.

**Must ask if missing**
- **Time period.** Relative periods count as given ("last month", "YTD", "Q3"). Follow-ups reuse the period already set.
- **Vertical and super partner:**
  - Neither mentioned → ask for both.
  - Vertical mentioned, no super partner → default to all super partners (don't ask).
  - Super partner mentioned, no vertical → ask for the vertical.

**Default and state it**
- **Comparison period** (when the question implies a comparison but doesn't name one, e.g. "how did signups do in September?"): the previous period of the same type. Month → previous calendar month; week → previous week; custom range → the same number of days just before it. For a partial period, compare to the average of last week
- **Traffic type:** the department's default traffic type from the routing table.
- **Incomplete current period:** say the period isn't complete and show data through the latest date available.

## Tableau call rules

- **No discovery calls when the skill already covers the question.** If the routing table and cards cover it, don't call `list-datasources`, `list-workbooks`, `list-projects`, `search-content` or `get-datasource-metadata`. LUIDs, field captions and valid values are already in the cards.
- **When discovery and metadata calls are allowed.** Only in these three cases:
  1. **Outdated card:** a query fails because a field in the card isn't found → one `get-datasource-metadata` call to recover.
  2. **Field not in the card:** the question needs a field the card doesn't list → one `get-datasource-metadata` call.
  3. **Data not in the routing table:** search for it (`search-content` or `list-datasources`, skipping ignored folders), then `get-datasource-metadata`, then query. Don't say the data can't be found until you've searched.

  Whenever one of these happens, prepare a report to the BI team so the skill can be updated.
- **No profiling queries.** The query tool's own guidance suggests running COUNT queries before the real query. Skip them: the cards describe each datasource's grain and size.
- **One aggregated query per question, 500 rows by default.** Always aggregate and filter. Set `limit` to 501: if 501 rows come back, the result was cut off, so show the first 500 and ask whether the user needs the complete data. If they do, run the query again with a higher limit.
- **Call budget: at most 10 Tableau calls per question,** counting every search, metadata, query and view call. Aim for one query. If more than 10 seem needed, tell the user what you would run and ask first.
- **Retries.** Retry a failing query at most once, and only after changing something. Tell the user in plain language why it's failing, ask clarification questions if needed, and offer to report it to the BI team.
- **Query-service limits.** No sets or combined fields, no groups in filters or calculations, one filter per field, no fiscal-date calculations. Details are in `references/known-issues.md`.

## Output template

Use this structure every time. Keep it short: the headline answers the question; the rest lets someone check it.

```
**<Headline: one sentence with the number, the change and the period>**

<Small table: only the rows that support the headline, about 10 rows or fewer>

**Receipt**
- Source: <datasource name> (or view: <view name>)
- Metrics: <standard metric name> = <plain-English definition>
- Filters: <date range · vertical · super partner · any other filters applied>
- Assumptions: <defaults applied>
- Tableau API calls: <n>
- Dig deeper: <dashboard link>
```

- **CSV:** attach the full result as a CSV when it has more than 10 rows, or when the user asks.
- **Chart:** add a simple line or bar chart only when it genuinely helps (e.g., a trend over many periods) or when the user asks.

## Out of scope

Decline clearly and point to the right place (see `references/fail-safe.md`):
- Creating, changing, publishing or deleting anything in Tableau.

## Reporting issues to the BI team

Reports are emails from the user's own Gmail to the BI team. Prepare one when:
- The user asks (e.g., "report this", "this number looks wrong").
- A discovery or metadata call was needed (see "Tableau call rules"), so a card or the routing table needs updating.
- A query still fails after max retries.
- A QA check fails and can't be resolved.

Follow `references/report-issue.md` for the steps and the email template. Always show the user the full email and send it only after they confirm.

## Reference files

Open only what the current step needs.

| File | Read when |
|---|---|
| `references/glossary.md` | Step 2: mapping the user's words to standard metrics and dimensions |
| `references/departments/<department>.md` | Steps 1–4: department vocabulary, defaults and approved views (paths in the routing table) |
| `references/datasources/<datasource-name>.md` | Steps 3–5: LUID, field captions, formulas and valid values (paths in the routing table) |
| `references/query-patterns.md` | Step 5: query templates |
| `references/known-issues.md` | Step 5: known data and tool quirks, before every query |
| `references/qa-template.md` | Step 6: checks before answering |
| `references/fail-safe.md` | Whenever you can't give a reliable answer |
| `references/report-issue.md` | Step 9: whenever a report to the BI team is needed |
