# Wise Funnel Diagnostic — Product Analytics Case Study

A two-part analytics case study examining the international transfer
funnel (Created → Funded → Transferred) for Wise, the UK fintech.

## Context

Completed as part of an interview process for a Product Analyst role
at Wise. The brief asks for (1) a review of a junior analyst's
speed-vs-conversion analysis and (2) a diagnostic of funnel
performance across a two-month event-level dataset spanning January
and February 2024.

## Approach

The analysis follows a layered diagnostic structure:

1. **Establish the headline** — build the funnel at the monthly level
   to see whether anything is moving and by how much.
2. **Localise the timing** — drop to daily granularity to find out
   when any movement began.
3. **Decompose** — when the headline moves, separate rate-change from
   mix-change by holding each segment constant.
4. **Find the smoking gun** — quantify the mix driver in a single
   number.
5. **Hunt for anomalies** — cross-tabulate dimensions to localise the
   biggest opportunity.
6. **Confirm structural vs episodic** — check whether anomalies span
   the full period.

Six SQL queries take the analysis from blended funnel down to a
single product anomaly worth fixing.

## Key findings

- The headline Created→Funded conversion drops 7 percentage points
  between January and February (49.1% → 42.7%).
- Decomposition shows this is a **customer mix shift, not a product
  regression** — within-segment conversion is flat (Existing ~60%,
  New ~25% in both months). New user acquisition grew 44%
  month-on-month, lifting the New share of the created cohort from
  35% to 47%.
- The most actionable anomaly: **New users on Android convert at
  13.7% vs 36.9% on iOS** at the pay-in step — a 2.7× gap. New
  Android users who do fund transfer downstream at the highest rate
  of any cohort (84.6%), isolating the issue squarely in the pay-in
  step. The gap is structural, present in both months.

## Repository structure

```
wise-funnel-case-study/
├── README.md
├── analysis.sql                          Six structured SQL queries
└── report/
    ├── wise_case_study_theodore_li.docx  Analysis report (3 pages)
    └── chart_data_for_sheets.csv         Underlying data for charts
```

## Running the SQL

The queries are written for and tested in CSVFiddle. Load the
source CSV as a table named `"raw data"` (with a space, so the name
must be quoted) and run the queries in order. Each query has a
commented header explaining what it answers and the expected finding.

## Note on data

The source dataset was provided by Wise as part of the case study and
is not included in this repository.
