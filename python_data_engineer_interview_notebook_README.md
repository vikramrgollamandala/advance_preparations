# Guide: Python Data Engineer Interview Notebook

This guide explains how to approach the exercises in `python_data_engineer_interview_notebook.ipynb`.

## Medium 1: Customer Revenue Summary

Question:
- Summarize paid orders by customer.

Step by step:
1. Filter to `status == "paid"`.
2. Group by `customer_id`.
3. Compute:
   - total paid amount
   - count of paid orders
   - mean paid order amount
4. Reset index if needed.
5. Sort descending by total paid amount.

What to explain:
- Why filtering happens before aggregation.
- Why grouped metrics usually produce one row per business key.

## Medium 2: Top SKU by Revenue

Question:
- Join orders and order items and compute SKU-level revenue for paid orders.

Step by step:
1. Filter orders to paid rows only.
2. Join paid orders to `order_items` on `order_id`.
3. Create line-item revenue as `qty * unit_price`.
4. Group by `sku`.
5. Aggregate revenue and total units sold.
6. Sort descending by revenue.

What to explain:
- Fact-to-detail join pattern.
- Why row multiplication can happen if join keys are not unique.

## Medium 3: Sessionization with pandas

Question:
- Build user sessions from event timestamps.

Step by step:
1. Sort by `user_id`, `event_ts`.
2. Use `groupby("user_id").shift(1)` to get the previous event time.
3. Compute time gaps.
4. Mark a new session when:
   - there is no previous event, or
   - the gap is greater than 30 minutes.
5. Convert the boolean session-start flag into a running session id per user with cumulative sum.
6. Group by `user_id`, `session_id`.
7. Aggregate:
   - min timestamp as start
   - max timestamp as end
   - count of events

What to explain:
- Why sorting is mandatory.
- Why this is a classic event-processing problem.
- How this would change for streaming.

## Medium 4: Rolling 2-Day Customer Spend

Question:
- Compute rolling spend over the last 2 days for each customer.

Step by step:
1. Filter to paid orders.
2. Sort by `customer_id`, `order_ts`.
3. Group by `customer_id`.
4. Use time-based rolling on `order_ts` with a 2-day window.
5. Sum `amount`.
6. Attach the rolling result back to the filtered rows.

What to explain:
- Difference between row-based rolling and time-based rolling.
- Why order is required for correct windows.

## Medium 5: Deduplicate by Latest Record

Question:
- Keep the latest row per `id`, using `updated_at` and `version`.

Step by step:
1. Sort by:
   - `id`
   - `updated_at`
   - `version`
2. Drop duplicates on `id`, keeping the last row.
3. Reset index.

What to explain:
- Why ordering defines the winning record.
- What tie-breakers do in CDC-style data.

## Hard 1: Incremental Merge Without SQL

Question:
- Simulate an upsert from incoming data into current data.

Step by step:
1. Concatenate current and incoming rows.
2. Sort by `business_key`, `updated_at`.
3. If needed, add source priority to let incoming win on ties.
4. Drop duplicates on `business_key`, keeping the last row.
5. Return merged results.

What to explain:
- How this models merge/upsert behavior.
- Why it is a simplified version of Delta/Iceberg merge semantics.

## Hard 2: Missing Inventory Dates per SKU

Question:
- Find missing dates in each SKU’s date range.

Step by step:
1. Group by `sku`.
2. For each group, find min and max snapshot date.
3. Build the full daily date range.
4. Compare against existing snapshot dates.
5. Return dates that are missing.

What to explain:
- Why this is useful for data completeness checks.
- How a vectorized vs loop-based solution trades readability and flexibility.

## Hard 3: Idempotent File Processor

Question:
- Process files once per checksum.

Step by step:
1. Maintain a structure keyed by file name or checksum history.
2. For each file event:
   - if same file and same checksum already processed, skip
   - if checksum changed, process
3. Track processed and skipped outputs.

What to explain:
- Difference between idempotency by file name and by content hash.
- Why this matters in event-driven pipelines.

## Hard 4: Streaming Top K Keys

Question:
- Track the top-k most frequent keys in a stream.

Step by step:
1. Count frequency with a dictionary or `Counter`.
2. Extract top-k using sorting or a heap.
3. Return the k most frequent keys.

What to explain:
- Exact vs approximate top-k.
- Why heaps help when `k` is small relative to input size.

## Hard 5: pandas Window Ranking per Day

Question:
- Rank customers by daily spend.

Step by step:
1. Filter to paid orders.
2. Create `order_date` from timestamp.
3. Aggregate spend per `order_date`, `customer_id`.
4. Rank within each day using dense rank.

What to explain:
- Why you aggregate before ranking.
- Difference between dense rank and row number.

## Hard 6: Data Quality Checks Framework

Question:
- Build reusable data checks.

Step by step:
1. Define a standard output schema for each check.
2. Implement checks as functions.
3. Pass a DataFrame and a list of check functions.
4. Collect outputs into a result list or DataFrame.

What to explain:
- Why standardizing check output matters.
- How to separate data validation from transformation logic.
