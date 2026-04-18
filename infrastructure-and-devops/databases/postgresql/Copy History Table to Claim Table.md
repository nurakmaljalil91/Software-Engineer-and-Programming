---
title: Copy History Table to Claim Table
category: postgresql
tags:
  - postgresql
created: 2026-03-28
updated: 2026-03-28
status: active
---
```sql
BEGIN;

-- (Optional) Quick sanity checks
-- How many OrderIds overlap between Histories and Claims?
SELECT COUNT(*) AS overlaps
FROM public."Histories" h
JOIN public."Claims"    c ON c."OrderId" = h."OrderId";

-- See any duplicates already in Claims (should be none ideally)
SELECT "OrderId", COUNT(*) 
FROM public."Claims"
GROUP BY "OrderId"
HAVING COUNT(*) > 1;

-- Copy from Histories -> Claims, skipping OrderIds already present in Claims.
-- If Histories has multiple rows for the same OrderId, keep the newest by UpdatedDate/CreatedDate.
WITH h_dedup AS (
  SELECT DISTINCT ON (h."OrderId")
         h."OrderId", h."UserName", h."CreatedOn", h."CompleteDate", h."StaffName",
         h."Building", h."Activities", h."Total", h."Status", h."RulpaymentStatus",
         h."UploadId", h."CreatedDate", h."CreatedBy", h."UpdatedDate", h."UpdatedBy",
         h."ClaimedDate", h."ClaimedNumber", h."InvoiceNumber"
  FROM public."Histories" h
  WHERE h."OrderId" IS NOT NULL
  ORDER BY h."OrderId", h."UpdatedDate" DESC NULLS LAST, h."CreatedDate" DESC
)
INSERT INTO public."Claims" (
  "OrderId","UserName","CreatedOn","CompleteDate","StaffName","Building",
  "Activities","Total","Status","RulpaymentStatus","UploadId",
  "CreatedDate","CreatedBy","UpdatedDate","UpdatedBy",
  "ClaimedDate","ClaimedNumber","InvoiceNumber"
)
SELECT
  d."OrderId", d."UserName", d."CreatedOn", d."CompleteDate", d."StaffName", d."Building",
  d."Activities", COALESCE(d."Total", 0), d."Status", d."RulpaymentStatus", d."UploadId",
  d."CreatedDate", d."CreatedBy", d."UpdatedDate", d."UpdatedBy",
  d."ClaimedDate", COALESCE(d."ClaimedNumber", 0), d."InvoiceNumber"
FROM h_dedup d
LEFT JOIN public."Claims" c ON c."OrderId" = d."OrderId"
WHERE c."Id" IS NULL
RETURNING "Id","OrderId";

-- If results look good:
COMMIT;
-- If anything looks off:
-- ROLLBACK;

```