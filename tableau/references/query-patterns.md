# Query patterns

Templates for the most common questions. Replace every `<...>` with the exact field caption, LUID or value from the datasource card. These are the parameters of the Tableau `query-datasource` and `get-view-data` tools.

## Contents
1. Metric by month, two periods (MoM)
2. Breakdown, top N
3. Ratio metric
4. Relative date range
5. Dashboard-exact number from an approved view
6. Rules for every query

## 1. Metric by month, two periods (MoM)

Example: "How did paid social signups do last month vs. the month before?"

```json
{
  "datasourceLuid": "<from card>",
  "query": {
    "fields": [
      { "fieldCaption": "<Date field>", "function": "TRUNC_MONTH", "fieldAlias": "Month", "sortDirection": "ASC", "sortPriority": 1 },
      { "fieldCaption": "<Signups field>", "function": "SUM", "fieldAlias": "Signups" }
    ],
    "filters": [
      { "field": { "fieldCaption": "<Date field>" }, "filterType": "QUANTITATIVE_DATE", "quantitativeFilterType": "RANGE", "minDate": "<first day of prior month>", "maxDate": "<last day of last month>" },
      { "field": { "fieldCaption": "<Channel field>" }, "filterType": "SET", "values": ["<value from card>"] }
    ]
  },
  "limit": 100
}
```

Compute the MoM change locally from the two rows.

## 2. Breakdown, top N

Example: "Top 10 super partners by signups last month."

```json
{
  "datasourceLuid": "<from card>",
  "query": {
    "fields": [
      { "fieldCaption": "<Super partner field>", "fieldAlias": "Super partner" },
      { "fieldCaption": "<Signups field>", "function": "SUM", "fieldAlias": "Signups", "sortDirection": "DESC", "sortPriority": 1 }
    ],
    "filters": [
      { "field": { "fieldCaption": "<Date field>" }, "filterType": "QUANTITATIVE_DATE", "quantitativeFilterType": "RANGE", "minDate": "<start>", "maxDate": "<end>" },
      { "field": { "fieldCaption": "<Super partner field>" }, "filterType": "TOP", "direction": "TOP", "howMany": 10, "fieldToMeasure": { "fieldCaption": "<Signups field>", "function": "SUM" } }
    ]
  }
}
```

Use the real super partner column from the card, not a Tableau group or set (see known issues).

## 3. Ratio metric

Pull the numerator and denominator as separate SUMs and divide locally. This keeps totals and subtotals correct (SUM/SUM) and lets you show both components if a number looks off.

```json
"fields": [
  { "fieldCaption": "<Date field>", "function": "TRUNC_MONTH", "fieldAlias": "Month" },
  { "fieldCaption": "<Numerator field>", "function": "SUM", "fieldAlias": "Numerator" },
  { "fieldCaption": "<Denominator field>", "function": "SUM", "fieldAlias": "Denominator" }
]
```

If the card defines the metric as a Tableau formula, it can also be passed as a calculation:

```json
{ "fieldCaption": "<Metric name>", "calculation": "SUM([<Numerator>]) / SUM([<Denominator>])", "fieldAlias": "<Metric name>" }
```

## 4. Relative date range

Example: "last 3 months". Relative date filters can't contain functions or calculations.

```json
{ "field": { "fieldCaption": "<Date field>" }, "filterType": "DATE", "periodType": "MONTHS", "dateRangeType": "LASTN", "rangeN": 3 }
```

TODO(BI): confirm whether "last N months" should include the current partial month.

## 5. Dashboard-exact number from an approved view

Use when the user wants the number exactly as shown on a dashboard. The view LUID and filter names are in the department file.

```json
{ "viewId": "<view LUID from department file>", "viewFilters": { "<Filter name>": "<value>" } }
```

## 6. Rules for every query

- Use exact field captions from the card; never guess a caption.
- One filter per field per query.
- Request every measure, dimension and period in one query rather than several small ones.
- Always aggregate; set `limit`.
- TODO(BI): add tested queries here as they're validated against dashboards.
