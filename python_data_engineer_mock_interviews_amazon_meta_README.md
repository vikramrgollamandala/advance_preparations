# Guide: Amazon / Meta Style Data Engineer Mock Interviews

This guide explains how to resolve the mock interview prompts in `python_data_engineer_mock_interviews_amazon_meta.ipynb`.

## How To Practice

For each mock:
1. Restate the problem.
2. Clarify assumptions.
3. Define the grain of the output.
4. Write the solution.
5. Explain runtime and edge cases.
6. Explain Spark / AWS productionization.

## Mock 1: Daily Revenue by Country

Approach:
1. Filter transactions to `paid`.
2. Join user country into transactions.
3. Create a daily date column from timestamp.
4. Group by date and country.
5. Sum revenue.
6. Sort by date and revenue.

What interviewers care about:
- handling missing user keys
- keeping the aggregation grain correct

## Mock 2: Top 2 Users per Country by Spend

Approach:
1. Filter paid transactions.
2. Join users.
3. Aggregate total spend per user within country.
4. Rank or sort within each country.
5. Keep top 2 rows per country.

What interviewers care about:
- tie handling
- top-N per partition pattern

## Mock 3: Sessionization

Approach:
1. Sort by user and timestamp.
2. Shift previous timestamp within user.
3. Compute gap.
4. Mark new sessions using 30-minute threshold.
5. Cumulative-sum session flags.
6. Aggregate start, end, and count.

What interviewers care about:
- window logic
- batch vs streaming implications

## Mock 4: Incremental Upsert

Approach:
1. Stack current and incoming data.
2. Sort by `txn_id`, `updated_at`.
3. Keep the latest row per `txn_id`.
4. If tie behavior matters, add source priority.

What interviewers care about:
- merge semantics
- idempotency
- late-arriving updates

## Mock 5: Funnel Metrics

Approach:
1. Normalize event names if needed.
2. Count distinct users at each stage.
3. Preserve stage ordering in output.

What interviewers care about:
- uniqueness logic
- repeated events
- late or missing stages

## Mock 6: Suspicious Transactions

Approach:
1. Filter to paid rows.
2. Get latest paid transaction per user.
3. Compute historical average paid amount per user.
4. Compare latest amount against 2x historical average.
5. Output a flag.

What interviewers care about:
- single-transaction users
- definition of “historical average”

## Mock 7: Ad Click-Through Rate

Approach:
1. Group by `ad_id`.
2. Count impressions.
3. Sum clicks.
4. Compute CTR.
5. Sort descending by CTR.

What interviewers care about:
- denominator correctness
- scale strategy for large event logs

## Mock 8: Data Quality Report

Approach:
1. Compute null rates.
2. Count duplicate keys.
3. Count invalid negative amounts.
4. Find foreign key misses.
5. Return a structured report with one row per check.

What interviewers care about:
- standardization of validation results
- integration into orchestration or alerting

## Mock 9: Latest Record per Business Key

Approach:
1. Sort by `order_id`, `updated_at`, `version`.
2. Keep the last row per `order_id`.

What interviewers care about:
- CDC reasoning
- deterministic tie breaking

## Mock 10: System Design

Strong answer structure:
1. ingestion layer
2. raw storage
3. schema management
4. transformation layer
5. curated serving tables
6. orchestration
7. quality checks
8. backfills
9. observability

AWS examples:
- S3 for raw and curated storage
- Glue or EMR / Spark for transforms
- Airflow or Step Functions for orchestration
- Athena / Redshift / Iceberg tables for querying
- CloudWatch + data quality alerts for monitoring
