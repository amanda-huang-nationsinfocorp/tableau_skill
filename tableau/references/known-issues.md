# Known issues and quirks

<!-- Owner: TODO · Last verified: YYYY-MM-DD -->

Check this file before every query. Add a row whenever a wrong answer, failed query or confusing question is traced to a cause, so the same thing doesn't happen twice. Most new rows come from issue reports emailed to the BI team (see `report-issue.md`); use the report's date in **Found**.

## Data issues

<!-- TODO(BI): add data quirks (late-arriving data, backfills, renamed partners, known gaps, test traffic, etc.). -->

| ID | Found | Datasource / metric | Symptom | Cause | Workaround | Status | Owner |
|---|---|---|---|---|---|---|---|
| D-001 | 2026-11-03 | SEM Daily Performance: MKT COST$, SPEND$, CPA$, CPC$, CPM$, CTR%, CLICKS#, IMPRESSIONS# | Cost metrics come back null (October 2026 was still null on Nov 3) | They return a value only when cost data is complete for every row in the group (`MIN([Cost Complete Flag]) = 1`) | Say the cost data isn't complete yet; don't report 0 | Open | TODO |

## Known differences between datasources

The same column name has different logic in different datasources. Until each is resolved, say in the receipt which datasource's version was used.

<!-- TODO(BI): for each, keep one calculation or split it into separate rows. -->

- **C1%, D1%:** ÷ M0# in Daily Core Business Overview, Paid Social AFID Quality and both SEM datasources; ÷ M0 FOR M1# in Core Operations & Conversion Master and Split Test.
- **CPA$:** MKT COST$ per M0# in Daily Core Business Overview; Spend ÷ M0 Count, only when cost data is connected, in Facebook Adset Performance; SEM cost ÷ M0 Count, only when cost is complete, in both SEM datasources.
- **MKT COST$:** Credco/RE marketing cost in Daily Core Business Overview and Paid Social AFID Quality; SEM cost in both SEM datasources.
- **SPEND$:** Spend in Facebook Adset Performance; MKT COST$ in SEM Daily Performance.
- **IMPRESSIONS#, CLICKS#, CTR%, CPC$, CPM$:** in SEM Daily Performance they return NULL unless cost data is complete (`MIN([Cost Complete Flag]) = 1`), and CPC$ and CPM$ use Sem Cost instead of Spend.
- **SMS CAPTURE#:** sum of Sms Capture Count in Daily Core Business Overview and Paid Social AFID Quality; named SMS CAPTURES# and counted as distinct phones on completed SMS leads in Core Operations & Conversion Master and Split Test.
- **LEAD ACCEPTED#:** sum of Lead Accepted Count in Daily Core Business Overview and Paid Social AFID Quality; distinct Lead Ids with Lead Submission Status = 'success' in Core Operations & Conversion Master.
- **RETURNS$% and REF,VOID,CB,RDR$%:** same formula under two names (Paid Social AFID Quality vs. Core Operations & Conversion Master).

## Environment

| ID | Issue | What to do |
|---|---|---|
| E-001 | Some datasources use live connections to Snowflake (see Refresh in `datasources.md`), so their queries run against the warehouse and can be slow. | Don't retry just because a query is slow. Keep queries aggregated and filtered. |
| E-002 | dbt runs once a day or less, so today's data isn't complete. | Report "Data through" in the receipt; don't treat today as a full day. TODO(BI): confirm refresh timing. |

## Tableau query-service limits

These come from Tableau's VizQL Data Service, which the `query-datasource` tool uses.

| ID | Limit | What to do |
|---|---|---|
| T-001 | Sets and combined fields can't be queried. | Use the underlying real column listed in the glossary. |
| T-002 | Groups can't be used in filters or referenced in calculations; bins can't be used in filters. | Filter on the real column (e.g., super partner from `dim_dimension_alpha`). |
| T-003 | Only one filter per field in a query. | Combine conditions into one filter (e.g., one SET filter with several values). |
| T-004 | Fiscal-date calculations, pass-through (RAWSQL), Python/R script and spatial calculations aren't supported. | Use calendar fields from `dim_calendar`. |
| T-005 | No hour/minute date aggregations. | Use DAY or coarser. |
| T-006 | Set, match and relative date filters can't contain functions or calculations. | Filter on plain fields. |
| T-007 | The query service works with published (and composable) data sources; workbook-embedded data sources work only in an interactive session, so logic that lives only inside a workbook can't be queried. | Use the published datasource from the routing table, or an approved view via `get-view-data`. |
| T-008 | Site-wide usage cap: 100 queries per hour per Creator license, shared by everyone. | Follow the call budget in SKILL.md. |
| T-009 | The query tool's built-in guidance suggests metadata and COUNT profiling calls before querying. | Skip them; the glossary and `datasources.md` cover fields and grain (SKILL.md overrides this). |
| T-010 | Passing a parameter value fails: the tool says the value isn't allowed even when it is (tested 2026-11-03 with `Intraday Baseline Period`). | Don't use parameters or calculations that depend on them; query the underlying fields (`query-patterns.md` pattern 9). |
| T-011 | Tableau calculations that are already aggregated (e.g. `M0#`, `CR%`, `MKT COST$`) fail with `AGG` or `SUM`. | Query them with no `function`; add a function only to database columns (`query-patterns.md` section 1). |
| T-012 | SET filter values must exist in the data; unknown values are rejected with suggestions. | Use exact values from the glossary; numbers for IDs, `true`/`false` for flags. |

Sources: [VizQL Data Service limitations](https://help.tableau.com/current/api/vizql-data-service/en-us/docs/vds_limitations.html), [VizQL Data Service introduction](https://help.tableau.com/current/api/vizql-data-service/en-gb/index.html).

## Prompt and answer issues

<!-- Questions Claude misread, metrics it confused, answers users flagged. -->

| ID | Found | Question | What went wrong | Fix (file changed) | Owner |
|---|---|---|---|---|---|
| P-001 | YYYY-MM-DD | TODO | TODO | TODO | TODO |
