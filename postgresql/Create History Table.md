---
title: Create History Table
category: postgresql
tags:
  - postgresql
created: 2026-03-28
updated: 2026-03-28
status: active
---

## Problem

Claim table have a very big data now. Need to move old data to history table to make the query much faster

## Solution

### Create History Table

```sql
BEGIN;

-- 1A) Create history table with the same columns, constraints & identity
CREATE TABLE IF NOT EXISTS public."ClaimsHistory"
(LIKE public."Claims" INCLUDING DEFAULTS INCLUDING CONSTRAINTS INCLUDING IDENTITY);

-- 1B) (Optional) Add the same helpful indexes on history
CREATE INDEX IF NOT EXISTS idx_claims_history_completedate
    ON public."ClaimsHistory" USING btree ("CompleteDate" ASC NULLS LAST);

CREATE INDEX IF NOT EXISTS idx_claims_history_orderid
    ON public."ClaimsHistory" USING btree ("OrderId" ASC NULLS LAST);

CREATE INDEX IF NOT EXISTS idx_claims_history_status
    ON public."ClaimsHistory" USING btree ("Status" ASC NULLS LAST);

CREATE INDEX IF NOT EXISTS idx_claims_history_username
    ON public."ClaimsHistory" USING btree ("UserName" ASC NULLS LAST);

COMMIT;

```

### Move 2022-2024 rows into history (Then delete from live)

```sql
BEGIN;

-- Define the window you want to archive
-- (CompleteDate >= 2022-01-01 and < 2025-01-01)
WITH to_move AS (
  SELECT *
  FROM public."Claims"
  WHERE "CompleteDate" >= DATE '2022-01-01'
    AND "CompleteDate" <  DATE '2025-01-01'
)
INSERT INTO public."ClaimsHistory"
SELECT * FROM to_move;

-- Optional sanity check before delete
-- SELECT COUNT(*) FROM public."Claims_History"
--   WHERE "CompleteDate" >= DATE '2022-01-01'
--     AND "CompleteDate" <  DATE '2025-01-01';

DELETE FROM public."Claims"
WHERE "CompleteDate" >= DATE '2022-01-01'
  AND "CompleteDate" <  DATE '2025-01-01';

COMMIT;
```

### Reclaim space and refresh stats

`VACUUM FULL` will lock the table; if you need online compaction, consider `pg_repack`. Otherwise:

```sql
VACUUM (ANALYZE) public."Claims"; -- If you need to reclaim disk immediately (locks table): -- VACUUM FULL (ANALYZE) public."Claims";
```

## (Optional) Create a read-all view

So old reports still “see everything” without changing queries:

```sql

CREATE OR REPLACE VIEW public."Claims_All" AS SELECT * FROM public."Claims" UNION ALL SELECT * FROM public."Claims_History";
```

---

## Notes & best practices

- The `INSERT … SELECT` + `DELETE` pattern ensures identical rows land in history (including `"Id"` values). Because the history table has its **own** identity sequence, future inserts into history won’t conflict.

- The archive filter uses dates instead of `EXTRACT(YEAR ...)` so it can leverage the `idx_claims_completedate` index efficiently.

- If you have long-running writes to `"Claims"`, you can move in smaller batches to reduce locking time (e.g., by month):

```sql
-- Example monthly batch 

BEGIN; 

INSERT INTO public."ClaimsHistory" 
SELECT * FROM public."Claims" 
WHERE "CompleteDate" >= DATE '2022-01-01'
	AND "CompleteDate" <  DATE '2022-02-01'; 
DELETE FROM public."Claims" 
	WHERE "CompleteDate" >= DATE '2022-01-01' 
		AND "CompleteDate" <  DATE '2022-02-01'; 
		
COMMIT;
```
---

## Bonus (future-proofing): yearly partitions

If you want this to be painless every year, consider converting to **partitioned** table on `"CompleteDate"` with yearly partitions (`2022`, `2023`, `2024`, `2025`, …). Then “archiving” is just detaching older partitions (or moving them into another schema). If you want, I can give you a step-by-step to migrate `"Claims"` to native partitioning with minimal downtime.