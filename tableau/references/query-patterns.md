# Query patterns

<!-- Owner: TODO · Last verified: 2026-11-03 -->

Ready-made requests for the Tableau `query-datasource` tool, so the first query is valid, costs one call and matches the dashboards. Each pattern uses a real datasource; take field names from the glossary (they differ by datasource) and LUIDs from `datasources.md`.

✓ = ran successfully on 2026-11-03 · ○ = not run yet

## Contents
1. Rules for every query
2. Trend or period comparison ✓
3. Ratio metrics ✓
4. Top N breakdown ✓
5. Department default filters
6. Relative dates ✓
7. CLTV with the maturity cutoff ○
8. Split test variants ✓
9. Intraday pace ✓
10. Dashboard-exact number ○
11. Row-level lists ○

## 1. Rules for every query

- **Field form depends on the column** (glossary Calculation column):
  - Tableau calculation (a formula, e.g. `M0#`, `CR%`, `MKT COST$`): **no `function`**. `AGG` and `SUM` both fail. `{ "fieldCaption": "M0#" }`
  - Database column (e.g. `M0 Count`): add the function for its Type in `metric-rules.md` section 3. `{ "fieldCaption": "M0 Count", "function": "SUM" }`
  - Dimension: no function. Dates take `TRUNC_DAY`, `TRUNC_WEEK`, `TRUNC_MONTH`, `TRUNC_QUARTER` or `TRUNC_YEAR`.
- **One query per question.** Every measure, dimension and period goes in the same query.
- **Always add "data through"** for the receipt: `{ "fieldCaption": "Data Through", "calculation": "MAX([<date field>])" }`. A field can't be queried twice, so this has to be a calculation with its own name.
- **Filters:**
  - One filter per field.
  - SET values must exist in the data; Tableau rejects unknown values and suggests close ones. Use values from the glossary: numbers for IDs (`110`, not `"110"`), `true` / `false` for flags.
  - Dates only (`YYYY-MM-DD`), no times or time zones.
  - No sets, combined fields, bins or groups in filters, and no functions in SET, MATCH or relative date filters (`known-issues.md`).
- **`limit: 501`** (see the call rules in SKILL.md).
- **Compute locally:** MoM changes, shares, ratios, lifts and waterfalls. The Tableau tool can't run table calculations.
- **Don't pass parameters.** Parameter values currently fail (`known-issues.md` T-010); query the underlying fields instead (pattern 9).
- **Errors** name the problem (wrong field name, value or function). Fix that one thing and retry once.

## 2. Trend or period comparison ✓

"Credco signups and visitors, September vs. October" (Daily Core Business Overview):

```json
{
  "datasourceLuid": "f1f99ee6-8227-4a70-84d0-13b5f6c8898c",
  "query": {
    "fields": [
      { "fieldCaption": "CALENDAR_DATE", "function": "TRUNC_MONTH", "fieldAlias": "Month", "sortDirection": "ASC", "sortPriority": 1 },
      { "fieldCaption": "M0#", "fieldAlias": "M0" },
      { "fieldCaption": "VISITORS#", "fieldAlias": "Visitors" },
      { "fieldCaption": "Data Through", "calculation": "MAX([CALENDAR_DATE])" }
    ],
    "filters": [
      { "field": { "fieldCaption": "CALENDAR_DATE" }, "filterType": "QUANTITATIVE_DATE", "quantitativeFilterType": "RANGE", "minDate": "2026-09-01", "maxDate": "2026-10-31" },
      { "field": { "fieldCaption": "VERTICAL_TYPE" }, "filterType": "SET", "values": ["credco"] }
    ]
  },
  "limit": 501
}
```

- One row per month. Use `TRUNC_WEEK` or `TRUNC_DAY` for weekly or daily trends.
- One date range covering both periods; compute the change locally.

## 3. Ratio metrics ✓

- Query the numerator and denominator and divide locally (CR% = M0# ÷ VISITORS# from pattern 2). Totals and subtotals stay correct.
- When the numerator can't be queried (e.g. CLTV in Daily Core Business Overview, whose Sale Amount columns are hidden), query the ratio and its denominator and recombine: total = Σ(ratio × denominator) ÷ Σ denominator.

## 4. Top N breakdown ✓

"Top 5 super partners by credco signups in October":

```json
{
  "datasourceLuid": "f1f99ee6-8227-4a70-84d0-13b5f6c8898c",
  "query": {
    "fields": [
      { "fieldCaption": "SUPER_PARTNER_ID_NAME", "fieldAlias": "Super partner" },
      { "fieldCaption": "M0#", "fieldAlias": "M0", "sortDirection": "DESC", "sortPriority": 1 }
    ],
    "filters": [
      { "field": { "fieldCaption": "CALENDAR_DATE" }, "filterType": "QUANTITATIVE_DATE", "quantitativeFilterType": "RANGE", "minDate": "2026-10-01", "maxDate": "2026-10-31", "context": true },
      { "field": { "fieldCaption": "VERTICAL_TYPE" }, "filterType": "SET", "values": ["credco"], "context": true },
      { "field": { "fieldCaption": "SUPER_PARTNER_ID" }, "filterType": "SET", "values": [182, 192], "exclude": true, "context": true },
      { "field": { "fieldCaption": "SUPER_PARTNER_ID_NAME" }, "filterType": "TOP", "direction": "TOP", "howMany": 5, "fieldToMeasure": { "fieldCaption": "M0#" } }
    ]
  },
  "limit": 501
}
```

- **Mark every other filter `"context": true`.** Tableau applies Top N and other dimension filters at the same time, so without context the top 5 is picked from all data.
- `fieldToMeasure` takes the same form as the field (no function for a calculation).
- **Leave out super partners 182 and 192** (cascades, `metric-rules.md` section 2) with the excluding filter above. This filter was added after the test run.

## 5. Department default filters

Add the routing table's default unless the user asks otherwise, and state it in the receipt.

| Department | Datasource | Filter | Status |
|---|---|---|---|
| Internal Marketing – SEM | SEM Daily Performance | `{ "field": { "fieldCaption": "Super Partner Id" }, "filterType": "SET", "values": [110] }` | ✓ |
| Internal Marketing – Paid Social | Facebook Adset Performance | `{ "field": { "fieldCaption": "TRAFFIC_TYPE" }, "filterType": "SET", "values": ["Paid"] }` and `{ "field": { "fieldCaption": "Platform" }, "filterType": "SET", "values": ["Facebook"] }` | ○ |
| External Marketing | Daily Core Business Overview | `{ "field": { "fieldCaption": "IS_EXTERNAL_MARKETING" }, "filterType": "SET", "values": [true] }` | ○ |
| Company Overview, Executive Team | Any shared datasource | None (all traffic) | — |

- SEM cost fields return null until cost data is complete (`known-issues.md` D-001). Say the cost isn't complete; never report it as 0.

## 6. Relative dates ✓

```json
{ "field": { "fieldCaption": "Calendar Date" }, "filterType": "DATE", "periodType": "MONTHS", "dateRangeType": "LAST" }
```

- `LAST` = last full period ✓ · `CURRENT` = this period · `TODATE` = period to date (MTD, YTD) · `LASTN` with `rangeN` = last N periods.
- `anchorDate` (`YYYY-MM-DD`) replaces today as the reference date.
- For a comparison across two periods (pattern 2), use one QUANTITATIVE_DATE range that covers both instead.
- TODO(BI): confirm whether "last N months" should include the current partial month.

## 7. CLTV with the maturity cutoff ○

"Monthly CLTV 30 for credco this year": end the date range N days before today (`metric-rules.md` section 4), and say where it was cut.

```json
{
  "datasourceLuid": "f1f99ee6-8227-4a70-84d0-13b5f6c8898c",
  "query": {
    "fields": [
      { "fieldCaption": "CALENDAR_DATE", "function": "TRUNC_MONTH", "fieldAlias": "Month", "sortDirection": "ASC", "sortPriority": 1 },
      { "fieldCaption": "CLTV 0-30$", "fieldAlias": "CLTV 30" },
      { "fieldCaption": "M0#", "fieldAlias": "M0" },
      { "fieldCaption": "Data Through", "calculation": "MAX([CALENDAR_DATE])" }
    ],
    "filters": [
      { "field": { "fieldCaption": "CALENDAR_DATE" }, "filterType": "QUANTITATIVE_DATE", "quantitativeFilterType": "RANGE", "minDate": "2026-01-01", "maxDate": "<today minus 30 days>" },
      { "field": { "fieldCaption": "VERTICAL_TYPE" }, "filterType": "SET", "values": ["credco"] }
    ]
  },
  "limit": 501
}
```

- For a total across months, recombine with M0# (pattern 3).

## 8. Split test variants ✓

"How is T1367 doing?" (Split Test Core uses database columns, so add `SUM`):

```json
{
  "datasourceLuid": "4ef2079c-0a23-494c-8468-9692b8025992",
  "query": {
    "fields": [
      { "fieldCaption": "Test Id" },
      { "fieldCaption": "Variant Id", "sortDirection": "ASC", "sortPriority": 1 },
      { "fieldCaption": "M0 Count", "function": "SUM", "fieldAlias": "M0" },
      { "fieldCaption": "Visitor Count", "function": "SUM", "fieldAlias": "Visitors" }
    ],
    "filters": [
      { "field": { "fieldCaption": "Test Id" }, "filterType": "SET", "values": ["T1367"] }
    ]
  },
  "limit": 501
}
```

- Returns one row per variant (`T1367A`, `T1367B`). The variant ending in A is the control.
- Compute CR (M0 ÷ Visitors) per variant and the lift vs. the control locally.

## 9. Intraday pace ✓

"How are signups pacing today?" (Intraday Hourly Conversion):

```json
{
  "datasourceLuid": "252b8246-2a2c-43cb-b5d1-71b77019b190",
  "query": {
    "fields": [
      { "fieldCaption": "Comparison Day", "sortDirection": "ASC", "sortPriority": 1 },
      { "fieldCaption": "M0 Count", "function": "SUM", "fieldAlias": "M0" }
    ],
    "filters": [
      { "field": { "fieldCaption": "Is Comparable Hour" }, "filterType": "SET", "values": [true] }
    ]
  },
  "limit": 501
}
```

- Returns Today, Yesterday and Same Weekday Last Week over the same hours. Pace = (Today − baseline) ÷ baseline.
- Add `Vertical Type` for a breakdown. Don't use `M0 Today Comparable` / `M0 Baseline Comparable`; they need a parameter (T-010).

## 10. Dashboard-exact number ○

When the user wants the number exactly as a dashboard shows it, use an approved view from the dashboard directory in `fail-safe.md`:

```json
{ "viewId": "<view LUID from fail-safe.md>", "viewFilters": { "<Filter name>": "<value>" } }
```

- `viewFilters` names must match the dashboard's filter names.

## 11. Row-level lists ○

Only when the user asks for a list (e.g. IDs). Query dimensions only, filter as tightly as possible, keep `limit: 501`, and attach the result as a CSV when it has more than 10 rows.
