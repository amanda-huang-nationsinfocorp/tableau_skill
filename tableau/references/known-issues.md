# Known issues and quirks

<!-- Owner: TODO · Last verified: YYYY-MM-DD -->

Check this file before every query. Add a row whenever a wrong answer, failed query or confusing question is traced to a cause, so the same thing doesn't happen twice.

## Data issues

<!-- TODO(BI): add data quirks (late-arriving data, backfills, renamed partners, known gaps, test traffic, etc.). -->

| ID | Found | Datasource / metric | Symptom | Cause | Workaround | Status | Owner |
|---|---|---|---|---|---|---|---|
| D-001 | YYYY-MM-DD | TODO | TODO | TODO | TODO | Open | TODO |

## Environment

| ID | Issue | What to do |
|---|---|---|
| E-001 | Dashboards use live connections to Snowflake, so every query runs against the warehouse and can be slow. | Don't retry just because a query is slow. Keep queries aggregated and filtered. |
| E-002 | dbt runs once a day or less, so today's data isn't complete. | Report "Data through" in the receipt; don't treat today as a full day. TODO(BI): confirm refresh timing. |

## Tableau query-service limits

These come from Tableau's VizQL Data Service, which the `query-datasource` tool uses.

| ID | Limit | What to do |
|---|---|---|
| T-001 | Sets and combined fields can't be queried. | Use the underlying real column listed in the card. |
| T-002 | Groups can't be used in filters or referenced in calculations; bins can't be used in filters. | Filter on the real column (e.g., super partner from `dim_dimension_alpha`). |
| T-003 | Only one filter per field in a query. | Combine conditions into one filter (e.g., one SET filter with several values). |
| T-004 | Fiscal-date calculations, pass-through (RAWSQL), Python/R script and spatial calculations aren't supported. | Use calendar fields from `dim_calendar`. |
| T-005 | No hour/minute date aggregations. | Use DAY or coarser. |
| T-006 | Set, match and relative date filters can't contain functions or calculations. | Filter on plain fields. |
| T-007 | The query service works with published (and composable) data sources; workbook-embedded data sources work only in an interactive session, so logic that lives only inside a workbook can't be queried. | Use the published datasource in the card, or an approved view via `get-view-data`. |
| T-008 | Site-wide usage cap: 100 queries per hour per Creator license, shared by everyone. | Follow the call budget in SKILL.md. |
| T-009 | The query tool's built-in guidance suggests metadata and COUNT profiling calls before querying. | Skip them when a card exists (SKILL.md overrides this). |

Sources: [VizQL Data Service limitations](https://help.tableau.com/current/api/vizql-data-service/en-us/docs/vds_limitations.html), [VizQL Data Service introduction](https://help.tableau.com/current/api/vizql-data-service/en-gb/index.html).

## Prompt and answer issues

<!-- Questions Claude misread, metrics it confused, answers users flagged. -->

| ID | Found | Question | What went wrong | Fix (file changed) | Owner |
|---|---|---|---|---|---|
| P-001 | YYYY-MM-DD | TODO | TODO | TODO | TODO |
