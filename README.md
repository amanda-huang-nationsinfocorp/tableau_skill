# tableau_skill

A Claude skill that answers ad hoc business questions from our Tableau Cloud data: consistent with our Snowflake/dbt data model, using standard metric definitions and as few Tableau API calls as possible.

**Status:** scaffold. Most files have `TODO` markers to fill in.

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
│       ├── departments/         One file per Tableau department folder
│       └── datasources/         One card per published datasource
├── templates/
│   └── datasource-card.md       Copy this to create a new datasource card
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
7. Answer with a headline, a small table and a receipt (plus a CSV above 10 rows).
8. At any step, report issues to the BI team by email.

Target: **0 discovery calls and 1 Tableau query** for a typical question.

## Maintaining the skill

- **Add a datasource:** copy `templates/datasource-card.md` to `tableau/references/datasources/<name>.md`, fill it in, then add its path to the routing table in `SKILL.md` and its metrics to `glossary.md`.
- **Add a department:** copy an existing file in `tableau/references/departments/`, then add a row to the routing table.
- **Keep it current:** each card and department file has an owner and a "last verified" date. Re-verify after dbt or Tableau model changes, then rerun the evals.
- **Log problems:** add every traced wrong answer or failed query to `known-issues.md`.
- **Keep links flat:** `SKILL.md` should link every reference file directly (Claude may only partly read files that are linked from other reference files).

## Changes from the design doc

The design doc is "Tableau Skill Design.xlsx" in the Claude project "Tableau Skill".

- Added `glossary.md` (used in the Workflow tab but missing from the structure).
- Added `evals/` and `templates/`.
- Renamed `qa_template.md` to `qa-template.md` to match the other file names.
- The routing table in `SKILL.md` lists department files and datasource cards directly, so no reference file depends on another.

## Open decisions

- Routing table: teams, topics, Tableau folders, default traffic types, datasource cards.
- CLTV maturity windows: add to the glossary and datasource cards.
- Rule for an incomplete current period.
- BI team email address and the steps for sending issue reports.
- How the skill is distributed to users.
