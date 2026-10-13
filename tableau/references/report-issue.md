# Reporting issues to the BI team

Reports are emails sent from the user's own Gmail through the Gmail connector, so the BI team can reply to the person directly. "Reporting issues to the BI team" in SKILL.md says when to prepare one; this file says how.

## Contents
1. Steps
2. Email template
3. If Gmail isn't connected

## 1. Steps

1. **Get the user's note.** If they haven't said what looks wrong, ask once, in one short question.
2. **Find the relevant part of the conversation.** You can't see text the user highlights in the app, so ask them to point to it (e.g., "the second answer") or paste it. Quote it exactly.
3. **Collect the technical details** from the conversation: the question, department, datasource or view (name and LUID), the query (JSON), filters and defaults applied, any Tableau error text, the number of Tableau calls, and the data-through date.
4. **Offer a link to the whole conversation (optional).** The user clicks **Share** (claude.ai on the web; not available in the mobile app), shares it with the BI team or anyone in the organization, and pastes the link. Shared links work only inside the organization, show a snapshot of the chat, and hide raw Tableau results, so the email must still include the query and the numbers.
5. **Attach the data.** If a query returned rows, attach them to the email as a CSV.
6. **Draft, show, send.** Create a Gmail draft to the BI address, show the user the subject and full body, and send it only after they confirm. If they'd rather send it themselves, give them the draft link instead.
7. **Confirm** in one line that the report went out, then carry on with the user's original question if you can.

## 2. Email template

Write the body in plain text, not Markdown (Gmail shows Markdown symbols as-is).

```
To: TODO(BI): BI team email address
Subject: [Tableau Skill] <Type>: <short summary>

What's wrong (reporter's words):
<note>

Question asked:
<the user's question>

Relevant answer (quoted):
<excerpt>

Technical details:
- Department: <routing table row>
- Source: <datasource or view name> (<LUID>)
- Metrics: <standard metric names>
- Filters and defaults: <period, vertical, super partner, traffic type, other defaults>
- Query: <query JSON>
- Tableau error: <error text, or "none">
- Tableau calls used: <n>
- Data through: <date>

Conversation link: <link, or "not provided">
```

**Types** (pick one):
- **Wrong number:** the user thinks a result is incorrect.
- **Missing data:** the data couldn't be found, even after searching Tableau.
- **Skill gap:** a discovery or metadata call was needed (outdated card, field not in the card, data not in the routing table).
- **Tableau error:** a query or view still fails after the retry.
- **Other:** anything else.

## 3. If Gmail isn't connected

Give the user the same subject and body as text to paste into an email to the BI team, and mention that connecting Gmail in Claude lets you send reports for them next time.
