# Glossary

<!-- Owner: TODO · Last verified: YYYY-MM-DD (columns pulled from every datasource in the Tableau "Data Sources" folder on 2026-10-27) -->

Maps the user's words to standard metric and dimension names. Use the standard name in the answer and receipt. Field captions for each datasource are on the datasource card.

## Contents
1. How to use · 2. Columns · 3. Aggregation rules · 4. Maturity rules

## 1. How to use
- Find the user's word in **Aliases**; use the **Standard name**.
- Alias listed under more than one metric → use the row for the routed department; if still unclear, ask.
- Word not listed → don't guess. Ask, and prepare a Skill gap report (`report-issue.md`).

## 2. Columns

Each column is defined once here, even when it appears in several datasources. A department's own version is a separate row named `<Metric> (<Department>)`. Type sets the aggregation (section 3).

**Column names.** Standard names are Tableau column names. The same column is spelled slightly differently across datasources: match ignoring case, `_` vs. space, a trailing `(Table name)`, and the `_TAG` suffix used in the Split Test datasource (`M0#_TAG` = `M0#`). `[ND]` means the column also exists in N-day windows (`Sale Amount [ND]` = `Sale Amount`, `Sale Amount 30D`, …); the N values are in Calculation.

**Meaning** is the plain-English business meaning. **Calculation** shows how Tableau calculates the column, using the Daily Core Business Overview formula (or the most common one when the column isn't there); divide-by-zero guards are left out. "Database column" means Tableau doesn't calculate it: the value comes straight from the table. Core Operations & Conversion Master and Split Test count orders one by one instead, e.g. `COUNTD(IF [Is M0] = 1 THEN [Order Id] END)` for M0#.

<!-- TODO(BI): fill Aliases with the words people use, including Sisense-era names, and fill each TODO(BI) meaning. -->

### Date and time

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Calendar Date | date, day | Calendar date (`dim_calendar`) | Database column | Dimension |
| Week Starting | week | First day of the week. week start day | Database column | Dimension |
| Month Starting | month | First day of the month | Database column | Dimension |
| Year Number | year | Year | Database column | Dimension |
| Month Name | | Month name | Database column | Dimension |
| Month Number | | Month number | Database column | Dimension |
| Week Number | | Week number | Database column | Dimension |
| Day Name | | Weekday name | Database column | Dimension |
| Day Of Week | | Weekday number | Database column | Dimension |
| Day Of Month | | Day of the month | Database column | Dimension |
| Day Of Year | | Day of the year | Database column | Dimension |
| Week Diff | | TODO(BI) | Database column | Dimension |
| Ecltv 1 Y Ready | | Calendar flag. TODO(BI) | Database column | Dimension |
| Order Date | | Order date | Database column | Dimension |
| Order Hour | Order Hour Ts | Order hour (Intraday Hourly Conversion) | Database column | Dimension |
| Logdate | | TODO(BI) | Database column | Dimension |
| Created Date | Created Datetime | TODO(BI) | Database column | Dimension |
| Reporting Date La | | TODO(BI) | Database column | Dimension |

### Traffic source

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Vertical Type | vertical | Business vertical (values include 'credco'). TODO(BI): list values | Database column | Dimension |
| Vertical Type Bin | | TODO(BI) | Database column | Dimension |
| Vertical Supar Light | | TODO(BI) | Database column | Dimension |
| Brand Id Name | brand, Brand | Brand ID and name | Database column | Dimension |
| Brand Id | | Brand ID | Database column | Dimension |
| Brand Name | | Brand name | Database column | Dimension |
| Brand Name Abbr | | Brand abbreviation | Database column | Dimension |
| Brand Identifier | | TODO(BI) | Database column | Dimension |
| Super Partner Id Name | super partner, Super Partner, traffic source | Super partner ID and name; traffic-source breakdown (`dim_dimension_alpha`) | Database column | Dimension |
| Super Partner Id | | Super partner ID. > 200 = external marketing; 110 = Internal_SEM | Database column | Dimension |
| Super Partner Name | | Super partner name | Database column | Dimension |
| IS_EXTERNAL_MARKETING | external marketing | True for external marketing traffic | `[Super Partner Id] > 200` | Dimension |
| Partner Id Name | partner, Partner | Partner ID and name | Database column | Dimension |
| Partner Id | | Partner ID | Database column | Dimension |
| Partner Name | | Partner name | Database column | Dimension |
| PNL_PARTNER_GROUP | Pnl Partner Group Credco, Pnl Partner Group Fcl, Pnl Partner Group Rto | TODO(BI) | Database column | Dimension |
| Affiliate Id | Affiliate Display | Affiliate ID | Database column | Dimension |
| Ad Id | ADID, Ad Id Display | Ad ID | Database column | Dimension |
| Ad Name | | Ad name | Database column | Dimension |
| Original Ad Id | | TODO(BI) | Database column | Dimension |
| Parent Adid | | TODO(BI) | Database column | Dimension |
| Traffic Type | | Traffic type (values include 'Paid') | Database column | Dimension |
| Channel Type | channel | Marketing channel (e.g., paid search, display) | Database column | Dimension |
| Engine Type | engine | Search engine (e.g., Google, Bing) | Database column | Dimension |
| Corporate | | TODO(BI) | Database column | Dimension |
| Landing Page | | Landing page | Database column | Dimension |
| Welcome Id | | TODO(BI) | Database column | Dimension |
| Preview Search Id | | TODO(BI) | Database column | Dimension |
| Reg Att Category Id | | TODO(BI) | Database column | Dimension |
| Is Active | | TODO(BI) | Database column | Dimension |
| Is Light | | TODO(BI) | Database column | Dimension |
| Is Dynweekly | | TODO(BI) | Database column | Dimension |
| Transparency | | TODO(BI) | Database column | Dimension |
| Verifi Prepaid Bypass Pct | | TODO(BI) | Database column | Dimension |
| Bbb Brand Grade | | TODO(BI) | Database column | Dimension |
| Offer Id | | Offer ID | Database column | Dimension |
| Agg Offer Id | | TODO(BI) | Database column | Dimension |

### Orders and conversion

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Order Id | | Order ID | Database column | Dimension |
| Customer Id | | Customer ID | Database column | Dimension |
| X Id | | TODO(BI) | Database column | Dimension |
| Device Type | | Device type | Database column | Dimension |
| DEVICE_PLATFORM | | TODO(BI) | Database column | Dimension |
| Bin First | | TODO(BI) | Database column | Dimension |
| Card Type First | | TODO(BI) | Database column | Dimension |
| Merchant Processor Id First | | TODO(BI) | Database column | Dimension |
| M0# | M0 Count, M0 Cnt, Is M0 | TODO(BI) | `SUM([M0 Count])` | Count |
| D0# | D0 Count, Is D0 | TODO(BI) | `SUM([D0 Count])` | Count |
| C0# | C0 Count, Is C0 | TODO(BI) | `SUM([C0 Count])` | Count |
| V0# | V0 Count, Is V0 | TODO(BI) | `SUM([V0 Count])` | Count |
| V0R Cnt | Is V0R | TODO(BI) | Database column | Count |
| M0 FOR M1# | M0 For M1 Count, Is M0 For M1 | TODO(BI) | `SUM([M0 For M1 Count])` | Count |
| M1# | M1 Count, Is M1 | TODO(BI) | `SUM([M1 Count])` | Count |
| M1.N# | M1 N Count, Is M1 N | TODO(BI) | `SUM([M1 N Count])` | Count |
| C1# | C1 Count, Is C1 | TODO(BI) | `SUM([C1 Count])` | Count |
| D1# | | TODO(BI) | `[M0 FOR M1#] - [M1#] - [C1#]` | Count |
| ATTEMPTS# | | TODO(BI) | `[M0#] + [D0#] + SUM([Casc Success Count]) + SUM([Casc Fail Count])` | Count |
| CR% | conversion rate, CVR | Conversion rate: M0s per visitor | `[M0#] / [VISITORS#]` | Ratio |
| ATTEMPT RATE% | | Attempts per visitor | `[ATTEMPTS#] / [VISITORS#]` | Ratio |
| D0% | | TODO(BI) | `[D0#] / [M0#]` | Ratio |
| C0% | | TODO(BI) | `[C0#] / [M0#]` | Ratio |
| V0% | | TODO(BI) | `[V0#] / [M0#]` | Ratio |
| M0 FOR M1% | | TODO(BI) | `[M0 FOR M1#] / [M0#]` | Ratio |
| M1% | | TODO(BI) | `[M1#] / [M0 FOR M1#]` | Ratio |
| M1.N% | | TODO(BI) | `[M1.N#] / [M0 FOR M1#]` | Ratio |
| C1% | | TODO(BI) | `[C1#] / [M0#]` (differs by datasource, see below) | Ratio |
| D1% | | TODO(BI) | `[D1#] / [M0#]` (differs by datasource, see below) | Ratio |
| M0_CASHAPP_COUNT | Is M0 Cashapp | TODO(BI) | Database column | Count |
| M0 CASHAPP% | | TODO(BI) | `SUM([M0 Cashapp Count]) / SUM([M0 Count])` | Ratio |
| M0_MOBILE_COUNT | Is M0 Mobile | TODO(BI) | Database column | Count |
| M0 MOBILE% | | TODO(BI) | `SUM([M0 Mobile Count]) / SUM([M0 Count])` | Ratio |
| Is M0 Prepaid | | TODO(BI) | Database column (0/1) | Flag |
| GOOD BIN TIER M0% | | TODO(BI) | `SUM([Good Bin M0 Count]) / [M0#]` | Ratio |
| GOOD BIN TIER ATTEMPTS% | | TODO(BI) | `(SUM([Good Bin M0 Count]) + SUM([Good Bin D0 Count]) + SUM([Good Bin Casc Success Count]) + SUM([Good Bin Casc Fail Count])) / [ATTEMPTS#]` | Ratio |
| Is Rmkt Ba | | TODO(BI) | Database column (0/1) | Flag |
| Is Rmkt Em | | TODO(BI) | Database column (0/1) | Flag |
| Is Rmkt Sms | | TODO(BI) | Database column (0/1) | Flag |
| Is Tel Sale | | TODO(BI) | Database column (0/1) | Flag |

### Cascade (CASC)

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| CASC ALL# | Casc All Count | TODO(BI) | `[CASC B#] + [CASC D#] + [CASC P#]` | Count |
| CASC B# | Casc B Count, Is Casc B | TODO(BI) | `SUM([Casc B Count])` | Count |
| CASC D# | Casc D Count, Is Casc D | TODO(BI) | `SUM([Casc D Count])` | Count |
| CASC P# | Casc P Count, Is Casc P | TODO(BI) | `SUM([Casc P Count])` | Count |
| CASC SUCCESS# | Casc Success Count, Is Casc Success | TODO(BI) | `COUNTD(IF [Is Casc Success] = 1 THEN [Order Id] END)` | Count |
| CASC FAIL# | Casc Fail Count, Is Casc Fail | TODO(BI) | `COUNTD(IF [Is Casc Fail] = 1 THEN [Order Id] END)` | Count |
| CASC ALL% | | TODO(BI) | `[CASC ALL#] / [M0#]` | Ratio |
| CASC B% | | TODO(BI) | `[CASC B#] / [M0#]` | Ratio |
| CASC D% | | TODO(BI) | `[CASC D#] / [M0#]` | Ratio |
| CASC P% | | TODO(BI) | `[CASC P#] / [M0#]` | Ratio |

### Revenue and CLTV

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Sale Amount [ND] | sales | Sale amount | Database column; N = 15, 30, 60, 90, 360 | Amount |
| Sale Transaction Count [ND] | | Number of sale transactions | Database column; N = 30, 60, 90, 360 | Count |
| CLTV 0-30$ | CLTV 30 | Customer lifetime value: 30-day sales per M0 | `SUM([Sale Amount 30D]) / [M0#]` | Ratio |
| CLTV 0-60$ | CLTV 60 | Customer lifetime value: 60-day sales per M0 | `SUM([Sale Amount 60D]) / [M0#]` | Ratio |
| CLTV 0-360$ | CLTV 360, 1-year CLTV | Customer lifetime value: 1-year sales per M0 | `SUM([Sale Amount 360D]) / [M0#]` | Ratio |
| CLTV ALL$ | | Customer lifetime value: all sales to date per M0 | `SUM([Sale Amount]) / [M0#]` | Ratio |
| NET CLTV 0-30$ | | 30-day CLTV after refunds, voids, chargebacks and RDRs | `(SUM([Sale Amount 30D]) - SUM([Refund Amount 30D]) - SUM([Void Amount 30D]) - SUM([Cb Amount 30D]) - SUM([Rdr Amount 30D])) / [M0#]` | Ratio |
| NET CLTV ALL$ | | CLTV to date after refunds, voids, chargebacks and RDRs | `(SUM([Sale Amount]) - SUM([Refund Amount]) - SUM([Void Amount]) - SUM([Cb Amount]) - SUM([Rdr Amount])) / [M0#]` | Ratio |

### Refunds and chargebacks

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Refund Amount [ND] | refunds, REFUNDS$ | Refunded amount | Database column; N = 30, 60, 90, 360. REFUNDS$ = `SUM([Refund Amount])` | Amount |
| Csr Refund Amount [ND] | | TODO(BI) | Database column; N = 30, 60, 90, 360 | Amount |
| Cdrn Refund Amount [ND] | | TODO(BI) | Database column; N = 30, 60, 90, 360 | Amount |
| Ethoca Refund Amount [ND] | | TODO(BI) | Database column; N = 30, 60, 90, 360 | Amount |
| Void Amount [ND] | | Voided amount | Database column; N = 30, 60, 90, 360 | Amount |
| Rdr Amount [ND] | | TODO(BI) | Database column; N = 30, 60, 90, 360 | Amount |
| Cb Amount [ND] | chargeback amount | Chargeback amount | Database column; N = 30, 60, 90, 360 | Amount |
| Cb Count [ND] | chargebacks, CBs, CB# | Chargeback count | Database column; N = 30, 60, 90, 360. CB# = `SUM([Cb Count])` | Count |
| Nof Count [ND] | | TODO(BI) | Database column; N = 30 | Count |
| CB#% | chargeback rate, CB rate | Chargeback rate (by count) | `SUM([Cb Count]) / SUM([Sale Transaction Count])` | Ratio |
| CB30#% | | Chargeback rate (by count), 30-day window | `SUM([Cb Count 30D]) / SUM([Sale Transaction Count 30D])` | Ratio |
| CB60#% | | Chargeback rate (by count), 60-day window | `SUM([Cb Count 60D]) / SUM([Sale Transaction Count 60D])` | Ratio |
| REFUND$% | | Refunds as a share of sales | `SUM([Refund Amount]) / SUM([Sale Amount])` | Ratio |
| RETURNS$% | REF,VOID,CB,RDR$% | Refunds, voids, chargebacks and RDRs as a share of sales | `(SUM([Refund Amount]) + SUM([Void Amount]) + SUM([Cb Amount]) + SUM([Rdr Amount])) / SUM([Sale Amount])` | Ratio |

### Marketing cost

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| MKT COST$ | marketing cost | Marketing cost | `SUM(IF [Vertical Type] = "credco" THEN ZN([Mkt Cost Credco]) ELSE ZN([Mkt Cost Re]) END)` (differs by datasource, see below) | Amount |
| MKT COST RE$ | Mkt Cost Re | Marketing cost, RE | `SUM([Mkt Cost Re])` | Amount |
| MKT COST CREDCO$ | Mkt Cost Credco | Marketing cost, Credco | `SUM([Mkt Cost Credco])` | Amount |
| Mkt Cost Int Re | | TODO(BI) | Database column | Amount |
| Mkt Cost Ext Re | | TODO(BI) | Database column | Amount |
| Mkt Cost Ext Credco | | TODO(BI) | Database column | Amount |
| CPA$ | CPA, cost per acquisition, Cpa Dollar | Cost per acquisition: marketing cost per M0 | `[MKT COST$] / [M0#]` (differs by datasource, see below) | Ratio |
| CPA RE$ | | Cost per acquisition, RE | `[MKT COST RE$] / [M0#]` | Ratio |
| CPA CREDCO$ | | Cost per acquisition, Credco | `[MKT COST CREDCO$] / [M0#]` | Ratio |
| CPA$ (PARTNER-BASED) | | Cost per acquisition (Paid Social AFID Quality version) | `SUM(IF [Vertical Type] = "credco" THEN [Mkt Cost Credco] ELSE [Mkt Cost Re] END) / SUM([M0 Count])` | Ratio |
| EPC$ | | TODO(BI) | `[MKT COST$] / [VISITORS#]` | Ratio |
| POST SUCCESS COST$ | | Cost of successful lead posts at per-partner rates | `SUM(IF [Partner Id] IN (948017, 948018, 948019) THEN [LEAD_POST_SUCCESS_COUNT] * 1.15 ELSEIF [Partner Id] = 948020 THEN [LEAD_POST_SUCCESS_COUNT] * 0.8 ELSE 0 END)` | Amount |
| Sales Score | | TODO(BI) | Database column | TODO |

### leads

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Jluvr | | TODO(BI). Counted by JLVUR# and JEDI VISITORS# | Database column | Dimension |
| Lead Id | | Lead ID | Database column | Dimension |
| Lead Status | | Lead status (values include 'completed') | Database column | Dimension |
| Lead Submission Status | | Lead submission status (values include 'success') | Database column | Dimension |
| Lead Type | | Lead type (SMS types: 'internal_sms', 'sms_ccai', 'sms_fhc', 'sms_frto') | Database column | Dimension |
| Phone | | Phone number on the lead | Database column | Dimension |
| Visitor Source Flag | | TODO(BI) | Database column | Dimension |
| VISITORS# | visitors, Visitor Count, Visitor Cnt | TODO(BI) | `SUM([Visitor Count])` | Count |
| JEDI VISITORS# | Jedi Visitor Count | TODO(BI) | `COUNTD(IF [Event Type] = 'visitor' THEN [Jluvr] END)` | Distinct count |
| SERVER_VISITOR_COUNT | | TODO(BI) | Database column | Count |
| POST SUCCESS# | lead post success | Successful lead posts | `SUM([LEAD_POST_SUCCESS_COUNT])` | Count |
| LANDED RATE% | | TODO(BI) | `[VISITORS#] / [POST SUCCESS#]` | Ratio |
| LEAD ACCEPTED# | Lead Accepted Count | Accepted leads | `SUM([Lead Accepted Count])` (differs by datasource, see below) | Count |
| LEAD CAPTURE# | | Completed leads | `COUNTD(IF [Lead Status] = 'completed' THEN [Lead Id] END)` | Count |
| SMS CAPTURE# | SMS CAPTURES#, Sms Capture Count | SMS captures | `SUM([Sms Capture Count])` (differs by datasource, see below) | Count |
| SMS CAPTURES% | | SMS captures per visitor | `[SMS CAPTURE#] / [VISITORS#]` | Ratio |


### Ad platform

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Platform | | Ad platform (values include 'Facebook', 'TikTok') | Database column | Dimension |
| Account Id | | Ad account ID | Database column | Dimension |
| Account Name | | Ad account name | Database column | Dimension |
| Campaign Id | | Campaign ID | Database column | Dimension |
| Campaign Name | | Campaign name | Database column | Dimension |
| Campaign Type | | Campaign type | Database column | Dimension |
| Adset Id | | Ad set ID | Database column | Dimension |
| Adset Name | | Ad set name | Database column | Dimension |
| Adset Attribution Method | | TODO(BI) | Database column | Dimension |
| PLATFORM_AD_ID | | Ad ID on the ad platform | Database column | Dimension |
| PLATFORM_AD_NAME | | Ad name on the ad platform | Database column | Dimension |
| Audience Type | | Audience type | Database column | Dimension |
| Creative Type | | Creative type | Database column | Dimension |
| SPEND$ | spend, Spend | Ad platform spend | `SUM([Spend])` (differs by datasource, see below) | Amount |
| IMPRESSIONS# | impressions, Impressions | Ad impressions | `SUM([Impressions])` (differs by datasource, see below) | Count |
| CLICKS# | clicks, Clicks | Ad clicks | `SUM([Clicks])` (differs by datasource, see below) | Count |
| CTR% | click-through rate, Ctr Pct | Click-through rate | `SUM([Clicks]) / SUM([Impressions])` (differs by datasource, see below) | Ratio |
| CPC$ | cost per click, Cpc Dollar | Cost per click | `SUM([Spend]) / SUM([Clicks])` (differs by datasource, see below) | Ratio |
| CPM$ | Cpm Dollar | Cost per 1,000 impressions | `SUM([Spend]) / SUM([Impressions]) * 1000` (differs by datasource, see below) | Ratio |
| Total Spend For Parent Adid | | TODO(BI) | Database column | Amount |
| Adset Count For Parent Adid | | TODO(BI) | Database column | Count |
| Campaign Count For Parent Adid | | TODO(BI) | Database column | Count |
| M0 Adset Allocated Raw Count | | TODO(BI) | Database column | Count |
| M0 Parent Adid Actual Count | | TODO(BI) | Database column | Count |
| M0 Remaining Units | | TODO(BI) | Database column | Count |
| Casc All Adset Allocated Raw Count | | TODO(BI) | Database column | Count |
| Casc All Parent Adid Actual Count | | TODO(BI) | Database column | Count |
| Casc All Remaining Units | | TODO(BI) | Database column | Count |

### Intraday pacing

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Comparison Day | | Intraday day being compared: 'Today' or the baseline day | Database column | Dimension |
| M0 Today Comparable | | M0s so far today, comparable hours only | `SUM(IF [Comparison Day] = 'Today' AND [Is Comparable Hour] THEN [M0 Count] END)` | Count |
| M0 Baseline Comparable | | M0s on the baseline day over the same hours | `SUM(IF [Comparison Day] = [Intraday Baseline Period] AND [Is Comparable Hour] THEN [M0 Count] END)`; Intraday Baseline Period = Yesterday or Same Weekday Last Week | Count |
| M0 vs Baseline % | | Today's M0 pace vs. the baseline day | `([M0 Today Comparable] - [M0 Baseline Comparable]) / [M0 Baseline Comparable]` | Ratio |
| Casc Count | | TODO(BI) | Database column | Count |
| CASC Today Comparable | | Casc Count so far today, comparable hours only | `SUM(IF [Comparison Day] = 'Today' AND [Is Comparable Hour] THEN [Casc Count] END)` | Count |
| CASC Baseline Comparable | | Casc Count on the baseline day over the same hours | `SUM(IF [Comparison Day] = [Intraday Baseline Period] AND [Is Comparable Hour] THEN [Casc Count] END)` | Count |
| CASC vs Baseline % | | Today's CASC pace vs. the baseline day | `([CASC Today Comparable] - [CASC Baseline Comparable]) / [CASC Baseline Comparable]` | Ratio |

### Split test and user journey

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Test Id | test | Split test ID (e.g., T1367) | Database column | Dimension |
| Variant | | TODO(BI) | Database column | Dimension |
| Variant Id | | Variant ID | Database column | Dimension |
| Variant Name | | Variant name | Database column | Dimension |
| Variant Group | | TODO(BI) | Database column | Dimension |
| Is Control | | Control variant flag | Database column | Dimension |
| Split Variant Id | | TODO(BI) | Database column | Dimension |
| Adid Based Test Id | | TODO(BI) | Database column | Dimension |
| Adid Based Test Status | | TODO(BI) | Database column | Dimension |
| Tag Id | | TODO(BI) | Database column | Dimension |
| Tag Ids | | TODO(BI) | Database column | Dimension |
| Tag Name | | TODO(BI) | Database column | Dimension |
| User Action | | User action (funnel step) | Database column | Dimension |
| Detail User Action | | TODO(BI) | Database column | Dimension |
| Event Type | | Event type (values include 'visitor') | Database column | Dimension |
| Fragment Id | | TODO(BI) | Database column | Dimension |
| Fragment Name | | TODO(BI) | Database column | Dimension |
| Fb Flowid | | TODO(BI) | Database column | Dimension |
| Fb Flowname | | TODO(BI) | Database column | Dimension |
| Sequence Number | | TODO(BI) | Database column | Dimension |
| Submitted Value | | TODO(BI) | Database column | Dimension |
| Source Type | | TODO(BI) | Database column | Dimension |
| JLVUR# | USER ACTION JLUVR#_TAG | TODO(BI) | `COUNTD([Jluvr])` | Distinct count |
| CR LIFT%_TAG | CR lift | Lift in CR% vs. the first row (first variant) | `(ZN([CR%_TAG]) - LOOKUP(ZN([CR%_TAG]), FIRST())) / ABS(LOOKUP(ZN([CR%_TAG]), FIRST()))` | Table calc |
| AR LIFT%_TAG | | Lift in ATTEMPT RATE% vs. the first row (first variant) | `(ZN([ATTEMPT RATE%_TAG]) - LOOKUP(ZN([ATTEMPT RATE%_TAG]), FIRST())) / ABS(LOOKUP(ZN([ATTEMPT RATE%_TAG]), FIRST()))` | Table calc |
| Waterfall% | | Share of first-step users reaching each step | `ZN([USER ACTION JLUVR#_TAG]) / LOOKUP(ZN([USER ACTION JLUVR#_TAG]), FIRST())` | Table calc |
| Dynamic Waterfall % | | Share of first-step (or previous-step) users reaching each step | `[JLVUR#] / LOOKUP([JLVUR#], FIRST())`; when Waterfall Base Mode = step-by-step, `[JLVUR#] / LOOKUP([JLVUR#], -1)` (first step = 1) | Table calc |

### Technical and helper fields

Workbook helpers, keys and data-status fields. Don't report them as metrics or dimensions; status fields may be needed as filters (see the datasource card).

AD ID Text Filter, Intraday AD_ID Text Filter, Parent ADID Text Filter, Calendar Date Text, Week Starting Text, Month Starting Text, Date Range Control, Yesterday, REPORTING PERIOD, Detail Reporting Period, DIMENSION 1, DIMENSION 2, DIMENSION 3, Exclude Calendar Day, Keep Non-Excluded Days, Is Comparable Hour, Is Reporting Date, Last Comparable Hour, Last Complete Hour, Latest Order Event At La, Model Refreshed At, Model Refreshed At La, Faas Updated Date Pst, Jedi Updated Date Pst, Cost Data Status, VISITOR_DATA_STATUS, DIMENSION_MAPPING_STATUS, Row Type, Row Key, FACT_ROW_KEY, Surrogate Key, ID, Tag Id Index

### Known differences between datasources

The same column name has different logic in different datasources. Until each is resolved, say in the receipt which datasource's version was used.

<!-- TODO(BI): for each, keep one calculation or split it into separate rows. -->

- **C1%, D1%:** ÷ M0# in Daily Core Business Overview, Paid Social AFID Quality and both SEM datasources; ÷ M0 FOR M1# in Core Operations & Conversion Master and Split Test.
- **CPA$:** MKT COST$ ÷ M0# in Daily Core Business Overview; Spend ÷ M0 Count, only when cost data is connected, in Facebook Adset Performance; SEM cost ÷ M0 Count, only when cost is complete, in both SEM datasources.
- **MKT COST$:** Credco/RE marketing cost in Daily Core Business Overview and Paid Social AFID Quality; SEM cost in both SEM datasources.
- **SPEND$:** Spend in Facebook Adset Performance; MKT COST$ in SEM Daily Performance.
- **IMPRESSIONS#, CLICKS#, CTR%, CPC$, CPM$:** in SEM Daily Performance they return NULL unless cost data is complete (`MIN([Cost Complete Flag]) = 1`), and CPC$ and CPM$ use Sem Cost instead of Spend.
- **SMS CAPTURE#:** sum of Sms Capture Count in Daily Core Business Overview and Paid Social AFID Quality; named SMS CAPTURES# and counted as distinct phones on completed SMS leads in Core Operations & Conversion Master and Split Test.
- **LEAD ACCEPTED#:** sum of Lead Accepted Count in Daily Core Business Overview and Paid Social AFID Quality; distinct Lead Ids with Lead Submission Status = 'success' in Core Operations & Conversion Master.
- **RETURNS$% and REF,VOID,CB,RDR$%:** same formula under two names (Paid Social AFID Quality vs. Core Operations & Conversion Master).

## 3. Aggregation rules

| Type | How to aggregate |
|---|---|
| Dimension | Group by or filter |
| Count, Amount | SUM |
| Ratio (rates, per-unit values, averages) | SUM(numerator) / SUM(denominator); never average a ratio |
| Distinct count | COUNTD; not additive, so query the total directly |
| Flag (0/1 on each order) | Count orders where the flag is 1: COUNTD(IF [flag] = 1 THEN [Order Id] END) |
| Table calc | Compares rows within one result (e.g., each variant vs. the first); pull the underlying metric and compute locally |

## 4. Maturity rules

| Metric | Matures after | Measured from | Rule |
|---|---|---|---|
| CLTV 0-30$, CLTV 0-60$, CLTV 0-360$, NET CLTV 0-30$ | N days (30, 60, 360; CLTV 0-360$ = one year) | Cohort date. TODO(BI): which column | Latest usable cohort date = today − N. If part of the period isn't mature, cut it at the last mature date and say so |

<!-- TODO(BI): confirm whether the same rule applies to the other N-day columns (Cb Count 30D, CB30#%, Refund Amount 30D, …) and whether `Ecltv 1 Y Ready` in DIM_CALENDAR marks mature dates. -->
