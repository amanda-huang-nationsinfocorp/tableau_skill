---
name: tableau
description: Answers ad hoc business-metric questions from the company's Tableau Cloud data (signups, conversions, revenue, chargebacks, super partner and vertical performance, marketing channels, split tests and other KPIs) using standardized metric definitions, the right datasource for each department, and as few Tableau calls as possible. Use this skill whenever someone asks for a number, trend, comparison or breakdown of a company KPI ("how did paid social signups do last month?", "chargeback rate by super partner", "MoM revenue"), mentions a Tableau dashboard, view or datasource, or names a team or department (internal marketing, external marketing, advertising, finance, billing, customer service, split test, company overview), even if they never say "Tableau".
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

1. **Identify the department.** Use the routing table below. Route by the question's topic first; use the user's own department only as a tie-breaker. If it's still unclear, ask once.
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

<!-- TODO(BI): fill in teams, topics, Tableau folders, default traffic types and datasource cards. One row per department. -->

| Department | Teams / aliases | Typical topics | Tableau folder | Default traffic type | Department file | Datasource cards |
|---|---|---|---|---|---|---|
| Internal Marketing | TODO | paid social, TODO | TODO | TODO | `references/departments/internal-marketing.md` | TODO |
| External Marketing | TODO | TODO | TODO | TODO | `references/departments/external-marketing.md` | TODO |
| Advertising | TODO | TODO | TODO | TODO | `references/departments/advertising.md` | TODO |
| Finance | TODO | TODO | TODO | TODO | `references/departments/finance.md` | TODO |
| Billing | TODO | chargebacks, TODO | TODO | TODO | `references/departments/billing.md` | TODO |
| Customer Service | TODO | TODO | TODO | TODO | `references/departments/customer-service.md` | TODO |
| Split Test | TODO | TODO | TODO | TODO | `references/departments/split-test.md` | TODO |
| Company Overview | TODO | company-wide KPIs, TODO | TODO | TODO | `references/departments/company-overview.md` | TODO |

Datasource cards live at `references/datasources/<datasource-name>.md`. List each card's full path in the table above so it can be opened directly.

**Default traffic type** is the traffic-type filter the department's dashboards apply by default. Use it unless the user asks for something else, and state it in the receipt.

**Ignored Tableau folders** (never use as a source): TODO (e.g., sandbox, personal, archive folders).

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
- Filters: <period · vertical · super partner · traffic type>
- Assumptions: <defaults applied>
- Data through: <latest date in the result>
- Tableau calls: <n>
- Dig deeper: <dashboard link>
```

- **CSV:** attach the full result as a CSV when it has more than 10 rows, or when the user asks.
- **Chart:** add a simple line or bar chart only when it genuinely helps (e.g., a trend over many periods) or when the user asks.

## Row-level data and forecasts

- **Row-level and personal data:** allowed when available in Tableau. The 500-row default still applies.
- **Forecasts:** allowed when the user wants one. Label every forecast **"Claude's estimate, not from Tableau"**.

## Out of scope

Decline clearly and point to the right place (see `references/fail-safe.md`):
- Creating, changing, publishing or deleting anything in Tableau.

## Reporting issues to the BI team

Reports go to the BI team by email from the user's Gmail. TODO(BI): BI team email address; finalize the sending steps.

**When to prepare a report**
- The user asks (e.g., "report this", "this number looks wrong").
- A discovery or metadata call was needed (see "Tableau call rules"), so a card or the routing table needs updating.
- A query still fails after one retry.

**What the report contains**
- **The user's note:** what's wrong, in their words.
- **The relevant part of the conversation, quoted.** You can't see text the user highlights in the app, so ask them to point to it (e.g., "the second answer") or paste it.
- **Technical details:** the question, datasource or view, the query, filters, any Tableau error, and the number of Tableau calls.
- **Optional link to the whole conversation:** the user creates it with the Share button (claude.ai on the web) and pastes it. Shared chats hide raw Tableau results, so the report itself must include the query and the numbers.

Show the user the full email and send it only after they confirm.

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
