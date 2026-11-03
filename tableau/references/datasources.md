# Datasources

<!-- Owner: TODO · Last verified: 2026-11-03 (names, LUIDs and tables pulled from Tableau Cloud 2026-10-27; live vs. extract checked 2026-11-03) -->

The published datasources in the Tableau `Data Sources` folder. Use the **LUID** as `datasourceLuid` in every query; no lookup call is needed. The **Code** is how the glossary's Datasources column refers to each datasource. **Refresh** says how current the data is; use it for the "data through" date in the receipt.

Not listed: the `Data Sources/Internal Data Sources` subfolder (building-block tables; see the TODO in SKILL.md) and the Composable Data Sources Test1 datasource.

| Code | Datasource | LUID | Grain | Refresh |
|---|---|---|---|---|
| COCM | Core Operations & Conversion Master | `24ef3fe0-6c07-48e2-879a-5ad345fb1e3c` | TODO(BI) | Live connection: always check and list out most recent timestamp|
| DCBO | Daily Core Business Overview | `f1f99ee6-8227-4a70-84d0-13b5f6c8898c` | TODO(BI) | Extract |
| FBA | Facebook Adset Performance | `4e0b660c-ae52-4ec3-9748-d0b1c094ff46` | TODO(BI) | Extract |
| IHC | Intraday Hourly Conversion | `252b8246-2a2c-43cb-b5d1-71b77019b190` | TODO(BI) | Live connection: always check and list out most recent timestamp|
| PSAQ | Paid Social AFID Quality | `e0353333-6f87-4276-869c-124788aa6f7b` | TODO(BI) | Extract |
| SEMA | SEM Affiliate Daily Performance | `67619f55-2109-465e-89ab-de18d5a21606` | TODO(BI) | Extract |
| SEM | SEM Daily Performance | `53a8f593-bfa2-47bf-9cfa-aed4a5a75c35` | TODO(BI) | Extract |
| ST | Split Test | `8bb0d0c6-869b-45ae-9263-1d100762c29c` | TODO(BI) | Live connection: always check and list out most recent timestamp|
| STC | Split Test Core | `4ef2079c-0a23-494c-8468-9692b8025992` | Daily × Test × Variant × ADID × Affiliate; rolling six months (Tableau description) | Extract: full refresh after the daily data jobs |
| STUJ | Split Test User Journey | `ed1d6f6a-399e-4cdd-a591-9c24314ac07b` | TODO(BI) | Extract |
| UAT | User Action Tracking | `6efa50a9-5582-4bdb-be57-660926f935cb` | TODO(BI) | Extract |

<!-- TODO(BI): fill Grain (one row per …) and Refresh (when new data lands). Tables behind each datasource, from Tableau, to help:
- COCM: AGG_CASC_RMKT_TEL_ORDERS, AGG_ORDERS, AGG_VISITORS, DIM_AFFILIATE, DIM_CALENDAR, DIM_DIMENSION_ALPHA, FCT_CPA, FCT_LEAD_ALPHA, FCT_MONGODB_USER_ACTION
- DCBO: OBT_DAILY_CORE_OPERATIONS_CONVERSION
- FBA: FCT_FACEBOOK_ADSET_ALLOCATED
- IHC: OBT_INTRADAY_HOURLY_CONVERSION
- PSAQ: OBT_PAID_SOCIAL_AFID_DAILY_PERFORMANCE
- SEMA: OBT_SEM_AFFILIATE_DAILY_PERFORMANCE
- SEM: OBT_SEM_DAILY_PERFORMANCE
- ST: AGG_CASC_RMKT_TEL_ORDERS, AGG_CASC_RMKT_TEL_ORDERS_TAG, AGG_ORDERS, AGG_ORDERS_TAG, AGG_VISITORS, DIM_CALENDAR, DIM_DIMENSION_ALPHA, DIM_TAG_MAPPINGS, FCT_LEAD_ALPHA_TAG, FCT_MONGODB_USER_ACTION, FCT_USER_ACTION_TRACKING_TAG, FCT_VISITORS_TAG
- STC: OBT_DAILY_SPLIT_TEST_CONVERSION
- STUJ: FCT_USER_ACTION_TRACKING_TAG
- UAT: FCT_USER_ACTION_TRACKING
-->
