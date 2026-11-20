# Card Transaction Fraud Analysis (SQL)

A MySQL analysis of 284,807 real card transactions from two days in September 2013 (ULB/Worldline dataset). Nine SQL scripts that explore where fraud concentrates, how big the losses are, and whether simple alert rules are worth running.

## What it does

The project answers three questions a fraud team would ask:
1. **Where does the money go?** — fraud by amount band, time of day, running totals
2. **How big is the loss?** — rates, medians, confidence intervals
3. **Can a simple rule help?** — four alert rules, tuned on day 1, graded on day 2

## Data

- **Name:** Credit Card Fraud Detection (ULB / Worldline)
- **Source:** OpenML dataset 1597
- **Licence:** Database Contents License (DbCL) v1.0
- **Size:** 284,807 transactions (492 frauds, 0.167%), two days
- **Columns:** Time (seconds), V1-V28 (anonymous PCA components), Amount (EUR), Class (0/1)

## Key findings

| Finding | Number |
|---------|--------|
| Fraud rate | 0.167% (473 clean frauds of 283,726) |
| Total fraud value | EUR 58,591 (clean) |
| Top 10% of frauds by size | 61.58% of all fraud euros |
| Median fraud amount | EUR 9.82 (vs EUR 22.00 genuine) |
| Quiet-hour fraud rate | 0.582% vs 0.135% in busy hours |
| Best rule precision on day 2 | 96.51% |
| Best rule recall on day 2 | 41.29% |
| Euros caught per alert | EUR 82.97 |

## SQL scripts (run in order)

| # | File | What it does |
|---|------|-------------|
| 1 | 01_create_schema.sql | Table with types, indexes, generated columns |
| 2 | 02_load_data.sql | LOAD DATA LOCAL INFILE, row count check |
| 3 | 03_data_quality_checks.sql | NULLs, ranges, duplicates, creates transactions_clean view |
| 4 | 04_fraud_overview.sql | Fraud rate, confidence interval, money, medians |
| 5 | 05_amount_bands.sql | Fraud by amount band using NTILE, cumulative loss |
| 6 | 06_time_patterns.sql | Per hour, per day, ranks, running totals |
| 7 | 07_window_functions.sql | Deciles, loss concentration, gaps between frauds |
| 8 | 08_rule_evaluation.sql | Four rules, thresholds from day 1, confusion matrix on day 2 |
| 9 | 09_kpi_summary.sql | One-row KPI view |

## SQL techniques used

- CREATE TABLE with types, indexes and generated stored columns
- LOAD DATA LOCAL INFILE
- GROUP BY / HAVING, CTEs (WITH), window functions (ROW_NUMBER, RANK, DENSE_RANK, NTILE, LAG, SUM() OVER)
- WITH RECURSIVE for threshold sweep
- CROSS JOIN for grading multiple rules at once
- CASE, UNION ALL, views

## Other tools

- **Python:** file download, runs SQL in order, stats checks (Mann-Whitney), charts (Matplotlib)
- **MySQL 8+:** local server

## Limitations

- Only two days from 2013
- V1-V28 are anonymous PCA components — no business meaning
- No card ID: cannot track per-card patterns
- No cost data, so break-even is in gross fraud euros
- Thresholds are midpoints, not optimised

## Dataset

ULB/Worldline Credit Card Fraud Detection. Database Contents License v1.0.
