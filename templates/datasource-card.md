# <Datasource name>

<!--
Template for a datasource card.
Copy to: tableau/references/datasources/<datasource-name>.md
Then add the card's path to the routing table in SKILL.md and its metrics to glossary.md.
List only the fields people actually use. If the card grows past ~100 lines, add a "Contents" list at the top.
-->

<!-- Owner: TODO · Last verified: YYYY-MM-DD -->

## Card info

| | |
|---|---|
| Datasource LUID | TODO |
| Tableau project (folder) | TODO |
| Departments | TODO |
| Connection | TODO (live to Snowflake / extract) |
| Source dbt models / tables | TODO (fact table, `dim_calendar`, `dim_dimension_alpha`) |
| Grain | TODO (one row per ...) |
| Typical size | TODO (rows per month; tells Claude how much to aggregate) |
| Refresh | TODO (dbt run timing) |

## Data model

- TODO: how the fact table relates to `dim_calendar` (dates) and `dim_dimension_alpha` (super partners / traffic source breakdown), and on which keys.
- Request only the fields the question needs; with relationships, Tableau queries only the tables those fields come from.

## Metrics

| Standard name | Aliases | Tableau formula | Plain English | Aggregation | Department overrides | Notes |
|---|---|---|---|---|---|---|
| TODO | TODO | `SUM([TODO])` | TODO | SUM | — | — |

## Dimensions and valid filter values

| Field caption | Meaning | Valid values / format | Notes |
|---|---|---|---|
| TODO (date) | Calendar date from `dim_calendar` | YYYY-MM-DD | Use TRUNC_MONTH for monthly |
| TODO (super partner) | Super partner from `dim_dimension_alpha` | TODO | Real column, not a Tableau group or set |
| TODO | TODO | TODO | TODO |

## Default filters and standard exclusions
- TODO

## Gotchas
- TODO (link to the matching row in known-issues.md)

## Tested query

A query that has been checked against the dashboard:

```json
TODO
```
