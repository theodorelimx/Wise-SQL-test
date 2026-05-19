# Task 2: Conversion Dynamics Analysis — SQL Package

## Purpose

This SQL package analyses the Wise transfer funnel (Created → Funded → Transferred)
to answer two questions:

1. Are there noticeable trends in conversion dynamics, and what is the root cause?
2. Are there any anomalies that require the product team's attention, and what
   could be happening in the customer experience to cause them?

## Approach

The analysis follows a layered diagnostic structure rather than running every
possible cut. The goal is to move from overall rate to root cause in the smallest
number of steps that still produce a defensible answer (80/20).

Step 1 — Establish the overall rate. Build the funnel at the monthly level to see
whether anything is moving and by how much.

Step 2 — Localise the timing. Drop to daily granularity to find out when any
movement began. A clean step-change on a calendar boundary usually points to a
product release or campaign launch.

Step 3 — Decompose. When the overall rate moves, the question is always: did the
rate change within each segment, or did the mix of segments change? These look
identical at the top level but mean opposite things in product terms. Hold each
dimension constant and recompute to separate the two.

Step 4 — Find the smoking gun. If decomposition points to a mix shift, quantify
the shift directly — usually a single number that explains the overall trend.

Step 5 — Hunt for anomalies. Cross-tabulate dimensions to find segment cells
that are far out of line with their siblings. The funnel is the right tool here
because anomalies localise to a specific step.

Step 6 — Confirm structural vs episodic. For any anomaly, check whether it is
present across the whole period or just one month, to distinguish a regression
from a long-standing issue.

## Files

- `README.md` — this file
- `analysis.sql` — six queries corresponding to the six steps above, each
  with a commented header explaining what it answers and what to look for

## How to run

The dataset is a single table loaded from the provided CSV. The table is
named `raw data` (with a space, so it must be quoted as `"raw data"` in SQL)
with columns: `event_name`, `dt`, `user_id`, `region`, `platform`,
`experience`.

The queries are written for SQLite (compatible with CSVFiddle) and can be run
in order. Each query is self-contained.

## Summary of findings (preview)

The overall Created→Funded conversion drops from roughly 50% in January to
43% in February. Decomposition shows that within each customer experience
segment (New, Existing) conversion is flat — the drop is fully explained by a
mix shift, as the share of New users in the created cohort rose from 35% to
47% on the back of a ~44% increase in New user acquisition. Funded→Transferred
is stable throughout.

The most actionable anomaly is the Created→Funded conversion of New users on
Android, which sits at ~14% versus ~37% on iOS and ~25% on Web — a 2-3x gap
for the same customer type. New Android users who do fund convert downstream
at the highest rate of any cohort, isolating the issue squarely in the pay-in
step on Android for new users. This pattern is present in both months,
indicating a structural product weakness rather than a regression.
