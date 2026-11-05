# Glossary

<!-- Owner: TODO · Last verified: YYYY-MM-DD (columns pulled from every datasource in the Tableau "Data Sources" folder on 2026-10-27) -->

Maps the user's words to standard metric and dimension names. Use the standard name in the answer and receipt. Each datasource's LUID, grain and refresh are in `datasources.md`.

## Contents
1. How to use · 2. Columns · 3. Verticals · 4. Super partners

## 1. How to use
- Find the user's word in **Aliases**; use the **Standard name**.
- Alias listed under more than one metric → use the row for the routed department; if still unclear, ask.
- Word not listed → don't guess. Ask, and prepare a Skill gap report (`report-issue.md`).
- **Verticals and super partners (sections 3 and 4):** matching isn't case sensitive (CO = co = Co). In filters, still use the value exactly as it appears in the table.

## 2. Columns

Each column is defined once here, even when it appears in several datasources. A department's own version is a separate row named `<Metric> (<Department>)`. Type sets the aggregation (`metric-rules.md`).

**Column names.** Standard names are Tableau column names. The same column is spelled slightly differently across datasources: match ignoring case, `_` vs. space, a trailing `(Table name)`, and the `_TAG` suffix used in the Split Test datasource (`M0#_TAG` = `M0#`). `[ND]` means the column also exists in N-day windows (`Sale Amount [ND]` = `Sale Amount`, `Sale Amount 30D`, …); the N values are in Calculation.

**Meaning** is the plain-English business meaning; † marks meanings taken from the BI team's Reporting Dictionary that haven't been confirmed (it can be outdated). **Calculation** shows how Tableau calculates the column, using the Daily Core Business Overview formula (or the most common one when the column isn't there); divide-by-zero guards are left out. "Database column" means Tableau doesn't calculate it: the value comes straight from the table. Core Operations & Conversion Master and Split Test count orders one by one instead, e.g. `COUNTD(IF [Is M0] = 1 THEN [Order Id] END)` for M0#.

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
| Ecltv 1 Y Ready | | If the data has ECLTV 1Y predicted. It needs to be CLTV 30 matured | Database column | Dimension |
| Order Date | | Order date | Database column | Dimension |
| Order Hour | Order Hour Ts | Order hour (Intraday Hourly Conversion) | Database column | Dimension |
| Logdate | | Date | Database column | Dimension |
| Created Date | Created Datetime | TODO(BI) | Database column | Dimension |
| Reporting Date La | | Date | Database column | Dimension |

### Traffic source

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Vertical Type | vertical | Line of business. Values and their names: section 3 | Database column | Dimension |
| Vertical Type Bin | | TODO(BI) | Database column | Dimension |
| Vertical Supar Light | | TODO(BI) | Database column | Dimension |
| Brand Id Name | brand, Brand | Brand ID and name | Database column | Dimension |
| Brand Id | | Brand (website) ID | Database column | Dimension |
| Brand Name | | Brand name | Database column | Dimension |
| Brand Name Abbr | | Brand abbreviation | Database column | Dimension |
| Brand Identifier | | TODO(BI) | Database column | Dimension |
| Super Partner Id Name | super partner, Super Partner, traffic source | Super partner ID and name; traffic-source breakdown (`dim_dimension_alpha`). Top partners and their aliases: section 4 | Database column | Dimension |
| Super Partner Id | | Super partner ID. > 200 = external marketing; 182 = CASC D; 192 = CASC B and CASC P | Database column | Dimension |
| Super Partner Name | | The actual partner company, e.g. Homes, VB, SkyRocket, Reply † | Database column | Dimension |
| IS_EXTERNAL_MARKETING | external marketing | True for external marketing traffic | `[Super Partner Id] > 200` | Dimension |
| Partner Id Name | partner, Partner | Partner ID and name | Database column | Dimension |
| Partner Id | | More detailed breakdown of the super partner | Database column | Dimension |
| Partner Name | | Name of the Partner Id | Database column | Dimension |
| PNL_PARTNER_GROUP | Pnl Partner Group Credco, Pnl Partner Group Fcl, Pnl Partner Group Rto | TODO(BI) | Database column | Dimension |
| Affiliate Id | Affiliate Display | Affiliate ID: tracks traffic below the ad ID; independent of ad ID and XID † | Database column | Dimension |
| Ad Id | ADID, Ad Id Display | Ad ID: the main unit for a traffic source and the unit split tests run on; passed in the sales-site URL † | Database column | Dimension |
| Ad Name | | Ad name | Database column | Dimension |
| Original Ad Id | | TODO(BI) | Database column | Dimension |
| Parent Adid | | The ad ID marketing partners use in their links; visible to external partners † | Database column | Dimension |
| Traffic Type | | Traffic type. Tableau values include 'Paid'; the dictionary lists SEM values Desktop Display, Desktop Search, Mobile Display, Mobile Search † | Database column | Dimension |
| Channel Type | channel | Marketing channel, e.g. SearchInternal, SearchExternal, CPAnetwork, Remarketing † | Database column | Dimension |
| Engine Type | engine | Search engine: Google, Bing, Yahoo † | Database column | Dimension |
| Corporate | | Corporation name: NIC, NSR, RPDC, HL, MRE, T&R, GRTO, BS, GIL † | Database column | Dimension |
| Landing Page | | First page customers see after clicking our link † | Database column | Dimension |
| Welcome Id | | TODO(BI) | Database column | Dimension |
| Preview Search Id | | TODO(BI) | Database column | Dimension |
| Reg Att Category Id | | RATCAT: each ad ID's registration setup (trial price, monthly price, reg page, confirmation page, welcome email) † | Database column | Dimension |
| Is Active | | TODO(BI) | Database column | Dimension |
| Is Light | | TODO(BI) | Database column | Dimension |
| Is Dynweekly | | TODO(BI) | Database column | Dimension |
| Transparency | | Traffic-quality score, 0–100, applied by channel type † | Database column | Dimension |
| Verifi Prepaid Bypass Pct | | TODO(BI) | Database column | Dimension |
| Bbb Brand Grade | | TODO(BI) | Database column | Dimension |
| Offer Id | | Offer ID | Database column | Dimension |
| Agg Offer Id | | TODO(BI) | Database column | Dimension |

### Orders and conversion

Signup funnel (M0, D0, C0, C1, M1, D1, M1.N): see `metric-rules.md`.

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Order Id | | Order ID | Database column | Dimension |
| Customer Id | | Customer ID | Database column | Dimension |
| X Id | | XID: tracks sales-site traffic at the most detailed level; independent of ad ID and affiliate ID † | Database column | Dimension |
| Device Type | | Device type | Database column | Dimension |
| DEVICE_PLATFORM | | TODO(BI) | Database column | Dimension |
| Bin First | | BIN (first 6 card digits) of the signup payment † | Database column | Dimension |
| Card Type First | | Card type of the signup payment: Visa, MC, Amex, Discover † | Database column | Dimension |
| Merchant Processor Id First | | TODO(BI) | Database column | Dimension |
| M0# | converted, signups, sign-ups, trial signups, sales, M0 Count, M0 Cnt, Is M0 | Successful trial signups: visitors who signed up for the trial. Cascades: see `metric-rules.md` | `SUM([M0 Count])` | Count |
| D0# | hard declines, D0 Count, Is D0 | Hard declines: signup attempts declined at signup (soft declines go to CASC D instead) | `SUM([D0 Count])` | Count |
| C0# | same-day cancels, C0 Count, Is C0 | Same-day cancels: M0s who cancel on the signup day (calendar day, PST †) | `SUM([C0 Count])` | Count |
| V0# | V0 Count, Is V0 | Static verifications: Credco customers verified successfully † | `SUM([V0 Count])` | Count |
| V0R Cnt | Is V0R | Reported static verifications: customers whose verification pixel fired before 3 AM the day after the order † | Database column | Count |
| M0 FOR M1# | M0 For M1 Count, Is M0 For M1 | M0s eligible for the first subscription bill † | `SUM([M0 For M1 Count])` | Count |
| M1# | retention, M1 Count, Is M1 | First bill paid: M0s who got through the trial and paid the first subscription. "Retention" usually means M1# but can mean M1.N#; default to M1# and say so in the receipt | `SUM([M1 Count])` | Count |
| M1.N# | M1 N Count, Is M1 N | Customers who keep paying after M1 (M2, M3, …) | `SUM([M1 N Count])` | Count |
| C1# | C1 Count, Is C1 | Later cancels: M0s who cancel after the signup day, before the first subscription bill | `SUM([C1 Count])` | Count |
| D1# | | First bill declined: M0s whose trial went fine but whose first subscription payment was declined | `[M0 FOR M1#] - [M1#] - [C1#]` | Count |
| ATTEMPTS# | | Signup attempts (submit on the registration page), including cascades † | `[M0#] + [D0#] + SUM([Casc Success Count]) + SUM([Casc Fail Count])` | Count |
| CR% | conversion rate, CVR | Conversion rate: M0s per visitor | `[M0#] / [VISITORS#]` | Ratio |
| ATTEMPT RATE% | | Attempts per visitor | `[ATTEMPTS#] / [VISITORS#]` | Ratio |
| D0% | | Hard declines per M0 | `[D0#] / [M0#]` | Ratio |
| C0% | | Share of M0s who cancel the same day | `[C0#] / [M0#]` | Ratio |
| V0% | | Share of M0s with a static verification † | `[V0#] / [M0#]` | Ratio |
| M0 FOR M1% | | Share of M0s eligible for the first bill | `[M0 FOR M1#] / [M0#]` | Ratio |
| M1% | | First-bill rate: share of eligible M0s who paid the first subscription | `[M1#] / [M0 FOR M1#]` | Ratio |
| M1.N% | | Share of eligible M0s who kept paying after M1 | `[M1.N#] / [M0 FOR M1#]` | Ratio |
| C1% | | Later-cancel rate | `[C1#] / [M0#]` (differs by datasource, see `known-issues.md`) | Ratio |
| D1% | | First-bill decline rate | `[D1#] / [M0#]` (differs by datasource, see `known-issues.md`) | Ratio |
| M0_CASHAPP_COUNT | Is M0 Cashapp | M0s whose first payment used a Cash App BIN † | Database column | Count |
| M0 CASHAPP% | | Share of M0s whose first payment used a Cash App BIN † | `SUM([M0 Cashapp Count]) / SUM([M0 Count])` | Ratio |
| M0_MOBILE_COUNT | Is M0 Mobile | M0s from a mobile device † | Database column | Count |
| M0 MOBILE% | | Share of M0s from a mobile device † | `SUM([M0 Mobile Count]) / SUM([M0 Count])` | Ratio |
| Is M0 Prepaid | | TODO(BI) | Database column (0/1) | Flag |
| GOOD BIN TIER M0% | | Share of M0s with a good BIN (tier 1 or 2) | `SUM([Good Bin M0 Count]) / [M0#]` | Ratio |
| GOOD BIN TIER ATTEMPTS% | | Share of signup attempts with a good BIN (tier 1 or 2) | `(SUM([Good Bin M0 Count]) + SUM([Good Bin D0 Count]) + SUM([Good Bin Casc Success Count]) + SUM([Good Bin Casc Fail Count])) / [ATTEMPTS#]` | Ratio |
| Is Rmkt Ba | | TODO(BI) | Database column (0/1) | Flag |
| Is Rmkt Em | email | Order from email remarketing † | Database column (0/1) | Flag |
| Is Rmkt Sms | | Order from SMS remarketing † | Database column (0/1) | Flag |
| Is Tel Sale | | TODO(BI) | Database column (0/1) | Flag |

### Cascade (CASC)

How cascades count in M0#: see `metric-rules.md`.

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| CASC ALL# | Casc All Count, Casc Count | All cascades: CASC B + CASC D + CASC P | `[CASC B#] + [CASC D#] + [CASC P#]` | Count |
| CASC B# | Casc B Count, Is Casc B | Cascades from bad BINs, tier 4 (super partner 192) | `SUM([Casc B Count])` | Count |
| CASC D# | soft declines, Casc D Count, Is Casc D | Cascades from soft declines (super partner 182) | `SUM([Casc D Count])` | Count |
| CASC P# | Casc P Count, Is Casc P | Cascades from bad BINs, tier 3 (super partner 192). Not prepaid | `SUM([Casc P Count])` | Count |
| CASC SUCCESS# | Casc Success Count, Is Casc Success | Successful cascade signups | `COUNTD(IF [Is Casc Success] = 1 THEN [Order Id] END)` | Count |
| CASC FAIL# | Casc Fail Count, Is Casc Fail | Failed cascade signups | `COUNTD(IF [Is Casc Fail] = 1 THEN [Order Id] END)` | Count |
| CASC ALL% | | Cascades per M0 | `[CASC ALL#] / [M0#]` | Ratio |
| CASC B% | | CASC B per M0 | `[CASC B#] / [M0#]` | Ratio |
| CASC D% | | CASC D per M0 | `[CASC D#] / [M0#]` | Ratio |
| CASC P% | | CASC P per M0 | `[CASC P#] / [M0#]` | Ratio |

### Revenue and CLTV

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Sale Amount [ND] | revenue, sales $ | Revenue from successful sale and capture transactions; ND = within N days of signup † | Database column; N = 15, 30, 60, 90, 360 | Amount |
| Sale Transaction Count [ND] | | Successful sale and capture transactions; ND = within N days of signup † | Database column; N = 30, 60, 90, 360 | Count |
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
| Csr Refund Amount [ND] | | Refunds given by customer service reps (CSR) † | Database column; N = 30, 60, 90, 360 | Amount |
| Cdrn Refund Amount [ND] | | Refunds issued by Verifi through the CDRN program † | Database column; N = 30, 60, 90, 360 | Amount |
| Ethoca Refund Amount [ND] | | Refunds issued through Ethoca's chargeback-prevention service † | Database column; N = 30, 60, 90, 360 | Amount |
| Void Amount [ND] | | Voided amount: refunds on the same day as the charge † | Database column; N = 30, 60, 90, 360 | Amount |
| Rdr Amount [ND] | | Refunds issued by Visa through Rapid Dispute Resolution (RDR) † | Database column; N = 30, 60, 90, 360 | Amount |
| Cb Amount [ND] | chargeback amount | Chargeback amount | Database column; N = 30, 60, 90, 360 | Amount |
| Cb Count [ND] | chargebacks, CBs, CB# | Chargebacks: customer disputes a charge through their bank, which refunds them and fines us † | Database column; N = 30, 60, 90, 360. CB# = `SUM([Cb Count])` | Count |
| Nof Count [ND] | | TODO(BI) | Database column; N = 30 | Count |
| CB#% | chargeback rate, CB rate | Chargeback rate: chargebacks per successful sale and capture transaction † | `SUM([Cb Count]) / SUM([Sale Transaction Count])` | Ratio |
| CB30#% | | Chargeback rate within 30 days of signup † | `SUM([Cb Count 30D]) / SUM([Sale Transaction Count 30D])` | Ratio |
| CB60#% | | Chargeback rate within 60 days of signup † | `SUM([Cb Count 60D]) / SUM([Sale Transaction Count 60D])` | Ratio |
| REFUND$% | | Refunds as a share of sales | `SUM([Refund Amount]) / SUM([Sale Amount])` | Ratio |
| RETURNS$% | REF,VOID,CB,RDR$% | Refunds, voids, chargebacks and RDRs as a share of sales | `(SUM([Refund Amount]) + SUM([Void Amount]) + SUM([Cb Amount]) + SUM([Rdr Amount])) / SUM([Sale Amount])` | Ratio |

### Marketing cost

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| MKT COST$ | marketing cost | Marketing cost, based on CPA, CPC or CPV; calculated separately for RE and Credco † | `SUM(IF [Vertical Type] = "credco" THEN ZN([Mkt Cost Credco]) ELSE ZN([Mkt Cost Re]) END)` (differs by datasource, see `known-issues.md`) | Amount |
| MKT COST RE$ | Mkt Cost Re | Marketing cost for RE brands: (internal CPA + external CPA) × M0 count † | `SUM([Mkt Cost Re])` | Amount |
| MKT COST CREDCO$ | Mkt Cost Credco | Marketing cost for Credco brands: (internal CPA + external CPA) × V0R count † | `SUM([Mkt Cost Credco])` | Amount |
| CPA$ | CPA, cost per acquisition, Cpa Dollar | Cost per acquisition: marketing cost per M0 | `[MKT COST$] / [M0#]` (differs by datasource, see `known-issues.md`) | Ratio |
| CPA RE$ | | Cost per acquisition for RE brands | `[MKT COST RE$] / [M0#]` | Ratio |
| CPA CREDCO$ | | Cost per acquisition for Credco brands | `[MKT COST CREDCO$] / [M0#]` | Ratio |
| CPA$ (PARTNER-BASED) | | Cost per acquisition (Paid Social AFID Quality version) | `SUM(IF [Vertical Type] = "credco" THEN [Mkt Cost Credco] ELSE [Mkt Cost Re] END) / SUM([M0 Count])` | Ratio |
| EPC$ | | Marketing cost per visitor; EPC is one of the external marketing campaigns | `[MKT COST$] / [VISITORS#]` | Ratio |
| POST SUCCESS COST$ | | Cost of successful lead posts at per-partner rates | `SUM(IF [Partner Id] IN (948017, 948018, 948019) THEN [LEAD_POST_SUCCESS_COUNT] * 1.15 ELSEIF [Partner Id] = 948020 THEN [LEAD_POST_SUCCESS_COUNT] * 0.8 ELSE 0 END)` | Amount |
| Sales Score | | TODO(BI) | Database column | TODO |

### leads

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Jluvr | | Unique visitor code, unique by brand, device and browser † | Database column | Dimension |
| Lead Id | | Lead ID | Database column | Dimension |
| Lead Status | | Lead status (values include 'completed') | Database column | Dimension |
| Lead Submission Status | | Lead submission status (values include 'success') | Database column | Dimension |
| Lead Type | | Lead type (SMS types: 'internal_sms', 'sms_ccai', 'sms_fhc', 'sms_frto') | Database column | Dimension |
| Phone | | Phone number on the lead | Database column | Dimension |
| Visitor Source Flag | | TODO(BI) | Database column | Dimension |
| VISITORS# | visitors, Visitor Count, Visitor Cnt | Unique browsers on the sales site, guest site and member-site login page in the period, credited to the last ad ID and affiliate ID (P2 visitors) † | `SUM([Visitor Count])` | Count |
| JEDI VISITORS# | Jedi Visitor Count | Visits from the JEDI system: unique Jluvr per day, ad ID, affiliate ID and device; no bot filtering or last-ad-ID credit, so usually higher than VISITORS# † | `COUNTD(IF [Event Type] = 'visitor' THEN [Jluvr] END)` | Distinct count |
| SERVER_VISITOR_COUNT | | Server Visits: visits from daily server logs, counted before users reach P2 or JEDI; unique by XID too and no bot filtering, so usually higher than VISITORS# † | Database column | Count |
| POST SUCCESS# | lead post success | Successful lead posts | `SUM([LEAD_POST_SUCCESS_COUNT])` | Count |
| LANDED RATE% | | TODO(BI) | `[VISITORS#] / [POST SUCCESS#]` | Ratio |
| LEAD ACCEPTED# | Lead Accepted Count | Agent leads accepted by an advertising partner † | `SUM([Lead Accepted Count])` (differs by datasource, see `known-issues.md`) | Count |
| LEAD CAPTURE# | | Completed leads | `COUNTD(IF [Lead Status] = 'completed' THEN [Lead Id] END)` | Count |
| SMS CAPTURE# | SMS CAPTURES#, Sms Capture Count | SMS captures | `SUM([Sms Capture Count])` (differs by datasource, see `known-issues.md`) | Count |
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
| SPEND$ | spend, Spend | Ad platform spend | `SUM([Spend])` (differs by datasource, see `known-issues.md`) | Amount |
| IMPRESSIONS# | impressions, Impressions | Ad impressions | `SUM([Impressions])` (differs by datasource, see `known-issues.md`) | Count |
| CLICKS# | clicks, Clicks | Ad clicks | `SUM([Clicks])` (differs by datasource, see `known-issues.md`) | Count |
| CTR% | click-through rate, Ctr Pct | Click-through rate | `SUM([Clicks]) / SUM([Impressions])` (differs by datasource, see `known-issues.md`) | Ratio |
| CPC$ | cost per click, Cpc Dollar | Cost per click | `SUM([Spend]) / SUM([Clicks])` (differs by datasource, see `known-issues.md`) | Ratio |
| CPM$ | Cpm Dollar | Cost per 1,000 impressions | `SUM([Spend]) / SUM([Impressions]) * 1000` (differs by datasource, see `known-issues.md`) | Ratio |

### Intraday pacing

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Comparison Day | | Intraday day being compared: 'Today' or the baseline day | Database column | Dimension |
| M0 Today Comparable | | M0s so far today, comparable hours only | `SUM(IF [Comparison Day] = 'Today' AND [Is Comparable Hour] THEN [M0 Count] END)` | Count |
| M0 Baseline Comparable | | M0s on the baseline day over the same hours | `SUM(IF [Comparison Day] = [Intraday Baseline Period] AND [Is Comparable Hour] THEN [M0 Count] END)`; Intraday Baseline Period = Yesterday or Same Weekday Last Week | Count |
| M0 vs Baseline % | | Today's M0 pace vs. the baseline day | `([M0 Today Comparable] - [M0 Baseline Comparable]) / [M0 Baseline Comparable]` | Ratio |
| CASC Today Comparable | | Casc Count so far today, comparable hours only | `SUM(IF [Comparison Day] = 'Today' AND [Is Comparable Hour] THEN [Casc Count] END)` | Count |
| CASC Baseline Comparable | | Casc Count on the baseline day over the same hours | `SUM(IF [Comparison Day] = [Intraday Baseline Period] AND [Is Comparable Hour] THEN [Casc Count] END)` | Count |
| CASC vs Baseline % | | Today's CASC pace vs. the baseline day | `([CASC Today Comparable] - [CASC Baseline Comparable]) / [CASC Baseline Comparable]` | Ratio |

### Split test and user journey

| Standard name | Aliases | Meaning | Calculation | Type |
|---|---|---|---|---|
| Test Id | test, split test, ab test | Split test ID (e.g., T1367) | Database column | Dimension |
| Variant Id | | Variant ID: control ends with A; challengers end with B onward | Database column | Dimension |
| Variant Name | | Variant name (control or challenger) | Database column | Dimension |
| Variant Group | | Variant A or B | Database column | Dimension |
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
| JLVUR# | USER ACTION JLUVR#_TAG | Unique visitors by Jluvr (brand, device and browser) † | `COUNTD([Jluvr])` | Distinct count |
| CR LIFT%_TAG | CR lift | Lift in CR% vs. the first row (first variant) | `(ZN([CR%_TAG]) - LOOKUP(ZN([CR%_TAG]), FIRST())) / ABS(LOOKUP(ZN([CR%_TAG]), FIRST()))` | Table calc |
| AR LIFT%_TAG | | Lift in ATTEMPT RATE% vs. the first row (first variant) | `(ZN([ATTEMPT RATE%_TAG]) - LOOKUP(ZN([ATTEMPT RATE%_TAG]), FIRST())) / ABS(LOOKUP(ZN([ATTEMPT RATE%_TAG]), FIRST()))` | Table calc |
| Waterfall% | | Share of first-step users reaching each step | `ZN([USER ACTION JLUVR#_TAG]) / LOOKUP(ZN([USER ACTION JLUVR#_TAG]), FIRST())` | Table calc |
| Dynamic Waterfall % | | Share of first-step (or previous-step) users reaching each step | `[JLVUR#] / LOOKUP([JLVUR#], FIRST())`; when Waterfall Base Mode = step-by-step, `[JLVUR#] / LOOKUP([JLVUR#], -1)` (first step = 1) | Table calc |

### Technical and helper fields

Workbook helpers, keys, data-status fields and columns used only inside other calculations. Don't report them as metrics or dimensions; status fields may be needed as filters (see `known-issues.md`).

AD ID Text Filter, Intraday AD_ID Text Filter, Parent ADID Text Filter, Calendar Date Text, Week Starting Text, Month Starting Text, Date Range Control, Yesterday, REPORTING PERIOD, Detail Reporting Period, DIMENSION 1, DIMENSION 2, DIMENSION 3, Exclude Calendar Day, Keep Non-Excluded Days, Is Comparable Hour, Is Reporting Date, Last Comparable Hour, Last Complete Hour, Latest Order Event At La, Model Refreshed At, Model Refreshed At La, Faas Updated Date Pst, Jedi Updated Date Pst, Cost Data Status, VISITOR_DATA_STATUS, DIMENSION_MAPPING_STATUS, Row Type, Row Key, FACT_ROW_KEY, Surrogate Key, ID, Tag Id Index, Week Diff, Variant, Total Spend For Parent Adid, Adset Count For Parent Adid, Campaign Count For Parent Adid, M0 Adset Allocated Raw Count, M0 Parent Adid Actual Count, M0 Remaining Units, Casc All Adset Allocated Raw Count, Casc All Parent Adid Actual Count, Casc All Remaining Units, Mkt Cost Int Re, Mkt Cost Ext Re, Mkt Cost Ext Credco

## 3. Verticals

Values exactly as they appear in the data (lowercase); use them as-is in filters. Checked in Daily Core Business Overview and SEM Daily Performance on 2026-11-05.

| Standard name | Aliases | Notes |
|---|---|---|
| credco | credit | Credit |
| foreclosure | foreclosures | Real estate |
| rto | rent to own | Real estate |
| foreclosure_rto | FORTO | Real estate |


- **Real estate (RE)** = foreclosure, rto and foreclosure_rto together. In cost metrics, RE means every vertical except credco (MKT COST$ uses Mkt Cost Re for anything that isn't credco).
- **Not every datasource has every vertical.** SEM Daily Performance has only credco, foreclosure and rto, and a filter with a value that isn't there fails (`known-issues.md` T-012). Filter real estate there with foreclosure and rto only.

## 4. Super partners

The 50 super partners with the most M0s, including 182 and 192. Together they have 99.9% of all M0s in Daily Core Business Overview (all data through 2026-11-04, checked 2026-11-05). **Standard name** is the value of `SUPER_PARTNER_ID_NAME`; the number at the start is the ID.

- **ID or alias in the table** → use that row.
- **Not in the table** → apply the abbreviation rules below, then find the partner in the data:
  - The result stays under 501 rows → group the main query by `SUPER_PARTNER_ID_NAME` and pick the rows afterwards (no extra call).
  - Otherwise → one query listing `SUPER_PARTNER_ID_NAME` first.
- **More than one match** → ask. **No match** → ask, and prepare a Skill gap report (`report-issue.md`).
- **Filter by ID** (a number, e.g. `[233]`), not by name.

**Abbreviation rules** (partners not in the table):
- The first few words, shortened: 441 Big Steps Marketing → Big Steps
- First letter of each word: Consumer Opportunity → CO, Zero Parallel → ZP.

| Standard name | Aliases | Notes |
|---|---|---|
| 100 NIC | default, catch all | |
| 101 NIC Login | default, catch all | |
| 110 Internal_SEM | SEM | Default for SEM |
| 111 Internal_Social | Social | |
| 113 Internal_Browser_Alerts | | |
| 114 Internal_SMS | SMS | |
| 120 Internal_SEO | SEO, search | |
| 130 Internal_Criteo | | |
| 131 Internal_Email | Email | |
| 141 Internal_Ancillary_Revenue |Internal Ancillary | |
| 182 Internal_New_Cascade_Declines | casc d| CASC D (`metric-rules.md` section 2) |
| 192 Internal_New_Cascade_Non-Declines | casc b and p, casc none decline | CASC B and CASC P (`metric-rules.md` section 2) |
| 201 LeadQual | | |
| 202 OPO | | |
| 206 LidoLabs | Lido | |
| 208 MediaLot LLC | MediaLot | |
| 210 Xanadu Marketing | Xanadu | |
| 224 Perform CB | | |
| 225 Realtor.com | Realtor | |
| 228 EPCVIP |  | |
| 233 Zero Parallel LLC | ZP | |
| 237 DOT818 | DOT | |
| 238 IT Media | | |
| 239 Flux Marketing Group | Flux | |
| 240 Cordless Media | | |
| 241 Express Revenue | | |
| 244 Leadsgate (Brainsome Ltd) | Leadsgate | |
| 246 B2 Direct | | |
| 248 Stop Go Networks | Stop Go | |
| 252 Consumer Opportunity | CO | |
| 302 HomeFinder | | |
| 305 ZIP Foreclosures | | |
| 306 US Lease Option | US Lease | |
| 401 RateSpecial | | |
| 406 MaxBounty | | |
| 417 Fluent | | |
| 441 Big Steps Marketing | Big Steps | |
| 442 FirstChoice.Credit | FirstChoice | |
| 478 RxMG | | |
| 479 Bright Horizons Media | | |
| 488 JD Atlantic | JD | |
| 490 eMedia Group | eMedia | |
| 494 Cash Network | | |
| 500 Obsidian | | |
| 503 Highwinds LLC | | |
| 504 Bume Intl  Bume | |
| 507 Billy.com Leadgen Network Organization | Billy, Billy.com | |
| 1011 PingYo | | |
| 1012 Market Bullet | | |
| 1013 Harman Interactive Marketing | Harman | |
