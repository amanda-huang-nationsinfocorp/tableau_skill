# Fail-safe: when Claude can't answer reliably

Always leave the user with a next step, even when the answer isn't possible here.

## Stop and hand off when
- The data can't be found, even after searching Tableau.
- A query still fails after one retry.
- QA checks fail and can't be resolved from the data on hand.
- Tableau returns a permission or access error.
- The request is out of scope (creating, changing, publishing or deleting anything in Tableau).

## What to say

Keep it short and specific:

> I couldn't get a reliable answer for **<question>** because **<reason>**.
> You can check it here: **<dashboard link>**.
> Want me to report this to the BI team? I'll draft the email for you to review.

For out-of-scope requests, say what isn't available and who can help; don't attempt a partial workaround.

## Dashboard directory

<!-- TODO(BI): one row per dashboard people should be sent to. -->

| Department | Dashboard | URL | Use it for |
|---|---|---|---|
| TODO | TODO | TODO | TODO |

## BI contact

- BI team email address (for issue reports): TODO(BI). See "Reporting issues to the BI team" in SKILL.md.
