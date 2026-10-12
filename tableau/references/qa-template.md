# QA checklist

Run these checks before answering. Most need no extra Tableau call. If a check fails, fix it from the data you already have, or say so plainly in the answer.

## 1. Did the query do what the receipt says?
- [ ] Filters match the receipt: period, vertical, super partner, traffic type.
- [ ] Every requested period is in the result; partial periods (current month, data after the last refresh) are flagged.
- [ ] The standard metric from the glossary/card was used, not a similarly named field.

## 2. Is the data internally consistent?
- [ ] Parts add up to the total (breakdown vs. overall), allowing for distinct counts, which don't add up.
- [ ] Ratios are SUM/SUM, not averages of ratios.
- [ ] No unexpected nulls, zeros or duplicate rows.
- [ ] Row count is plausible for the grain and filters.

## 3. Does it make business sense?
- [ ] The order of magnitude is in the usual range. TODO(BI): add typical ranges for key metrics to the glossary or cards.
- [ ] Large swings (TODO(BI): threshold, e.g. ±30% MoM) are called out. Explain them only with a known issue or a calendar reason (e.g., month length, holiday); otherwise say the cause isn't known.
- [ ] If the number looks off and an approved view exists, suggest the user compare it with the dashboard. Don't spend an extra call unless they ask.

## 4. Is the answer traceable?
- [ ] Receipt complete: source, metric definitions, filters, assumptions, data-through date, number of Tableau calls, dashboard link.
