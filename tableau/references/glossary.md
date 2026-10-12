# Glossary: standard metric and dimension names

<!-- Owner: TODO · Last verified: YYYY-MM-DD -->

How to use this file: find the user's word in the **Aliases** column, use the **Standard name** in the answer, and open the listed datasource card for the exact formula and field caption.

## Metrics

<!-- TODO(BI): one row per metric. Aliases = every way people say it (incl. Sisense-era names). -->

| Standard name | Aliases | Plain-English definition | Aggregation | Datasource card | Departments |
|---|---|---|---|---|---|
| Signups | signups, sign-ups, TODO | TODO | SUM | TODO | TODO |
| Chargeback rate | CB rate, TODO | TODO | SUM(numerator) / SUM(denominator) | TODO | Billing, TODO |
| TODO | TODO | TODO | TODO | TODO | TODO |

## Dimensions

| Standard name | Aliases | Meaning | Source |
|---|---|---|---|
| Date | day, month, period | Calendar date | `dim_calendar` |
| Super partner | partner, traffic source, TODO | Traffic-source breakdown | `dim_dimension_alpha` |
| Vertical | TODO | TODO | TODO |
| Channel | paid social, TODO | TODO | TODO |

## Aggregation rules

- **Counts and amounts:** SUM.
- **Ratios and rates:** SUM(numerator) / SUM(denominator). Never average a ratio across rows, periods or segments.
- **Distinct counts:** COUNTD. Not additive: don't add them across periods or segments; query the total directly.
- **Averages:** recompute from the underlying sum and count.

## Department overrides

If a department uses a standard name to mean something different, give that version its own standard name here (for example, "Signups (Finance)") instead of letting the meaning change by department. The receipt then shows exactly which version was used.

| Standard name | Department | How it differs |
|---|---|---|
| TODO | TODO | TODO |
