# tableau_skill

A Claude skill that quickly answers ad hoc business questions from our Tableau Cloud data: consistent with our Snowflake/dbt data model, using standard metric definitions and as few Tableau API calls as possible.

**Version:** 0.1.0 · **Status:** scaffold; most files have `TODO` markers to fill in.

**Owner:** Amanda Huang

## Layout

```
tableau_skill/
├── README.md                    This file
├── tableau/                     The skill itself (this folder is what gets installed)
│   ├── SKILL.md                 When the skill activates, workflow, routing table, call rules, output template
│   └── references/
│       ├── glossary.md          Metric aliases → standard names, aggregation rules
│       ├── query-patterns.md    Query templates for common questions
│       ├── known-issues.md      Data, environment and Tableau quirks
│       ├── qa-template.md       Checks before answering
│       ├── fail-safe.md         What to do when Claude can't answer; dashboard directory
│       ├── report-issue.md      How Claude drafts and sends issue reports to the BI team
│       ├── departments/         One file per Tableau department folder
│       └── datasources/         One card per published datasource
├── templates/
│   └── datasource-card.md       Template for creating a new datasource card
└── evals/
    └── evals.json               Test questions and expected answers
```

`templates/` and `evals/` sit outside `tableau/` so they aren't installed with the skill.

## How the skill answers a question

1. Route to a department (routing table in `SKILL.md`).
2. Map the user's words to standard metrics (`glossary.md`).
3. Ask only if something that changes the answer is missing; otherwise state defaults.
4. Use the datasource card by default, or an approved view when it answers the exact question; search Tableau if the data isn't in the skill.
5. Run one aggregated query.
6. Compute changes and ratios locally; run the QA checks.
7. Answer with a headline, a small table and a receipt (plus a CSV if above 10 rows).
8. At any step, user can report issues to the BI team by email (`report-issue.md`).

Target: **0 discovery calls and 1 Tableau query** for a typical question.

## Testing

Test questions live in `evals/evals.json`. Run them in a new Claude chat with the skill turned on, before every merge to `main` and after changes to the dbt models or Tableau datasources.

| Requirement | Type | Passes when |
|---|---|---|
| Correct answer | Hard | The numbers match the dashboard for the same period and filters |
| Right datasource | Hard | The receipt names the expected datasource or view |
| Speed | Soft | The answer arrives within about 1 minute (not counting time waiting for the user's replies) |
| Tableau calls | Soft | Fewer than 3 calls in total |
| Discovery calls | Soft | None |

- **Pass:** every requirement met.
- **Pass with notes:** hard requirements met, a soft one missed. OK to merge; note the miss in the pull request.
- **Fail:** a hard requirement missed. Don't merge.

Each test question should name its expected datasource and the dashboard numbers to compare against.

## Maintaining the skill

1. **Weekly check.** A scheduled task compares Tableau with the skill every week, looking for new, renamed or removed datasources, dashboards and fields.
2. **Email alert.** If anything changed, the BI team gets an email listing the changes and the skill files they affect.
3. **Update the skill** on a new branch:
   - **New datasource:** copy `templates/datasource-card.md` to `tableau/references/datasources/<name>.md` and fill it in. Then add its path to the routing table in `SKILL.md` and its metrics to `glossary.md`.
   - **New department:** copy an existing file in `tableau/references/departments/`, then add a row to the routing table.
   - **Changed or removed items:** update or remove the affected cards and routing table rows, and update each file's Last verified date.
   - Run the tests (see "Testing").
4. **Merge to `main`.** Once a BI team member approves, the merge syncs to Claude and everyone gets the update in their next session (see "Releasing an update").

## Best practices

- **New branch for every change.** Never commit directly to `main`; every merge needs approval from a BI team member.
- **Naming:** lowercase words joined by hyphens for all files and folders. Name datasource cards after the datasource (`daily-core-business-overview.md`) and department files after the Tableau folder (`internal-marketing.md`).
- **Fill in `TODO(BI)` markers.** They mark content the BI team still has to supply. Search for `TODO` before a release.
- **Keep every file under 500 lines.** Give any reference file over 100 lines a table of contents at the top, so Claude sees its full scope even when it reads only the start.
- **Keep links flat.** Link every reference file directly from `SKILL.md`; Claude may only partly read files that are linked from other reference files.
- **Define each metric once.** The glossary holds the standard name and aliases; the datasource card holds the formula. Point to them instead of repeating definitions.
- **Use one term per concept.** For example, always "datasource card", never "card" in one file and "data sheet" in another.
- **Never commit passwords, tokens or API keys.**

## Releasing an update

Not in use yet: this starts once the repo syncs to our Claude organization as a plugin (see "Open decisions").

1. Create a new branch and make changes there. Make sure to run tests before opening pull requests
2. Open a pull request and merge to `main` once a BI team member approves.
3. The merge syncs to Claude; everyone gets the new version in their next session.

## Open decisions
- All the TODOs
- CLTV maturity windows: add to the glossary and datasource cards.
- BI team email address for issue reports (a shared inbox or group works best).
- Distribution: make the repo private and restructure it into the plugin layout for GitHub sync.
- Weekly Tableau check: build the scheduled task and its email alert.
