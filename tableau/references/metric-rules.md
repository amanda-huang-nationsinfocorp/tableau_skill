# Metric rules

<!-- Owner: TODO · Last verified: 2026-11-05 -->

How to count our metrics correctly. Standard names, aliases and formulas are in `glossary.md`; this file says how to combine and read them. Apply it when clarifying (step 3), querying (step 5) and computing (step 6).

## Contents
1. Signup funnel · 2. Cascades and M0# · 3. Aggregation rules · 4. Maturity rules

## 1. Signup funnel

- Visitors attempt to sign up. A successful trial signup is **M0**; a hard decline is **D0**; a soft decline becomes a cascade (**CASC D**, section 2), and can later become M0 for super partner 182. Some bins are cascade bins, and they are automatically blocked from becoming M0 and sent to super partner 192. If sign up is successful, it become M0 for super partner 192
- After signup: **C0** = cancels the same day; **C1** = cancels later (before the first bill); **M1** = gets through the trial and pays the first subscription; **D1** = trial was fine but the first subscription payment was declined.
- **M1.N** = keeps paying after M1 (M2, M3, …).
- BIN tiers 1–2 are good BINs; tiers 3–4 are bad BINs.
- Cashapp are generally bad bins 

## 2. Cascades and M0#

Cascades are signups we don't accept normally (soft declines and bad BINs) but give a separate chance to sign up. Each cascade starts as a regular super partner's signup attempt, but the order is recorded as an M0 under super partner 182 (CASC D) or 192 (CASC B and CASC P).

- **A super partner's M0# (core M0#)** doesn't include the cascades it generated; those are in its CASC D#, CASC B# and CASC P#.
- **A vertical's, brand's or the company's M0#** includes 182 and 192, so every cascade is counted once. Don't add CASC ALL# to it.
- **Super-partner breakdowns and rankings:** leave 182 and 192 out, and say so in the receipt (`query-patterns.md` pattern 4).

## 3. Aggregation rules

In a query, a Tableau calculation (Calculation shows a formula) takes no `function`; only database columns take the function below (`query-patterns.md`).

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
| CLTV 0-30$, CLTV 0-60$, CLTV 0-360$, NET CLTV 0-30$ | N days (30, 60, 360; CLTV 0-360$ = one year) | Cohort date, usually order date | Latest usable cohort date = today − N. If part of the period isn't mature, cut it at the last mature date and say so |

<!-- TODO(BI): confirm whether the same rule applies to the other N-day columns (Cb Count 30D, CB30#%, Refund Amount 30D, …) and whether `Ecltv 1 Y Ready` in DIM_CALENDAR marks mature dates. -->
