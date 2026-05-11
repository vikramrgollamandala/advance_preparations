# Guide: 30-Day Data Engineer Plan Notebook

This guide explains how to use `python_data_engineer_30_day_plan_notebook.ipynb`.

## Purpose

This notebook is not just a coding workbook. It is a structured practice plan for:
- data cleaning
- SQL-style thinking
- joins and windows
- pipeline construction
- sessionization
- incremental updates
- data quality
- scaling and production reasoning

## How To Work Through It

For every day or section:
1. Read the explicit questions first.
2. State assumptions in plain language.
3. Solve the code prompt.
4. Explain runtime and memory.
5. Explain how it would scale to Spark and AWS.

## Week 1

### Data exploration
Resolution approach:
1. Inspect shapes and dtypes.
2. Count nulls.
3. Look for duplicates and suspicious values.
4. Separate blocking issues from informational issues.

### Data cleaning
Resolution approach:
1. Normalize strings consistently.
2. Convert timestamps and numeric columns explicitly.
3. Decide whether null handling should drop, fill, or quarantine.

### Deduplication
Resolution approach:
1. Decide the correct business key.
2. Decide the winning record based on timestamp.
3. Add tie-breaker logic where needed.

### Mini pipeline
Resolution approach:
1. Define clear stages.
2. Keep each stage deterministic.
3. Return intermediate outputs for observability.

## Week 2

### GroupBy and aggregations
Resolution approach:
1. Filter out invalid rows first.
2. Group at the correct grain.
3. Name aggregated columns explicitly.

### Transform
Resolution approach:
1. Use `transform` when you want group metrics repeated at row level.
2. Use `agg` when you want one row per group.

### SQL mapping
Resolution approach:
1. Identify whether the SQL pattern is group, join, or window.
2. Translate each part cleanly instead of writing one oversized pandas expression.

## Week 3

### Joins
Resolution approach:
1. Identify fact and dimension tables.
2. Check join key uniqueness.
3. Track unmatched keys.

### Window functions
Resolution approach:
1. Sort correctly before cumulative or ranking logic.
2. Decide partition key.
3. Decide ordering column.

### Top N
Resolution approach:
1. Sort within groups.
2. Take top rows with `groupby().head(n)`.
3. Be explicit about tie handling.

### Metrics pipeline
Resolution approach:
1. Clean inputs.
2. Produce aggregate outputs.
3. Produce monitoring outputs.
4. Add anomaly or quality checks.

## Week 4

### Sessionization
Resolution approach:
1. Sort by user and time.
2. Shift timestamps within user.
3. Compute gap.
4. Mark session starts.
5. Cumulative-sum the flags.
6. Aggregate sessions.

### Incremental processing
Resolution approach:
1. Concatenate old and new records.
2. Sort by business key and freshness column.
3. Keep latest record per key.
4. Recompute downstream aggregates as needed.

### Data quality
Resolution approach:
1. Define which rules are mandatory.
2. Return structured results.
3. Keep checks reusable.

### Performance thinking
Resolution approach:
1. Avoid `apply` unless necessary.
2. Prefer vectorized operations.
3. Escalate to Spark when memory or execution time becomes the bottleneck.

### Mock interview and system thinking
Resolution approach:
1. Solve the data problem.
2. Explain edge cases.
3. Explain production design:
   - storage
   - orchestration
   - retries
   - monitoring
   - backfills

## What Good Answers Look Like

A strong answer includes:
- correct transformation logic
- clear business-grain reasoning
- careful handling of nulls and duplicates
- runtime explanation
- a credible Spark/AWS production path
