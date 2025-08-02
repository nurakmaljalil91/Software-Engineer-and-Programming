## Overview

Here is the corrected partition definition:

### Rename the existing table:


```sql
ALTER TABLE public."Claims" RENAME TO "Claims_old";
```

### Create the new partitioned table with `CompleteDate` as the partition key

```sql
CREATE TABLE public."Claims" (     
	"Id" bigint NOT NULL,     
	"OrderId" text,     
	"UserName" text,     
	"CreatedOn" date,     
	"CompleteDate" date,     
	"StaffName" text,     
	"Building" text,     
	"Activities" text,     
	"Total" numeric DEFAULT 0.0 NOT NULL,     
	"Status" text,     
	"RulpaymentStatus" text,     
	"UploadId" text,     
	"CreatedDate" timestamp with time zone NOT NULL,     
	"CreatedBy" text,     
	"UpdatedDate" timestamp with time zone NOT NULL,     
	"UpdatedBy" text,     
	"ClaimedDate" date,     
	"ClaimedNumber" integer DEFAULT 0 NOT NULL 
) PARTITION BY RANGE ("CompleteDate");
```

### Create the partitions for each year with adjusted upper boundary dates:

```sql
CREATE TABLE public."Claims_2022" PARTITION OF public."Claims" FOR VALUES FROM ('2022-01-01') TO ('2023-01-01');  

CREATE TABLE public."Claims_2023" PARTITION OF public."Claims" FOR VALUES FROM ('2023-01-01') TO ('2024-01-01');  

CREATE TABLE public."Claims_2024" PARTITION OF public."Claims" FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');  

CREATE TABLE public."Claims_2025" PARTITION OF public."Claims" FOR VALUES FROM ('2025-01-01') TO ('2026-01-01');
```

### Transfer data from the old table to the new partitioned table


```sql
INSERT INTO public."Claims" SELECT * FROM public."Claims_old";
```

### Drop the old table if no longer needed:


```sql
DROP TABLE public."Claims_old";
```

### Create indexes on the partitions as needed:


```sql
CREATE INDEX idx_claims_2022_orderid ON public."Claims_2022"("OrderId"); 
CREATE INDEX idx_claims_2022_username ON public."Claims_2022"("UserName"); 
CREATE INDEX idx_claims_2023_orderid ON public."Claims_2023"("OrderId"); 
CREATE INDEX idx_claims_2023_username ON public."Claims_2023"("UserName"); 
CREATE INDEX idx_claims_2024_orderid ON public."Claims_2024"("OrderId"); 
CREATE INDEX idx_claims_2024_username ON public."Claims_2024"("UserName"); 
CREATE INDEX idx_claims_2025_orderid ON public."Claims_2025"("OrderId"); 
CREATE INDEX idx_claims_2025_username ON public."Claims_2025"("UserName");
```

### Full Example Script

Here’s the complete script with the adjusted partition boundaries:

```sql
-- Step 1: Rename the existing table 
ALTER TABLE public."Claims" RENAME TO "Claims_old";  

-- Step 2: Create the new partitioned table with CompleteDate as the partition key 
CREATE TABLE public."Claims" (     
	"Id" bigint NOT NULL,     
	"OrderId" text,     
	"UserName" text,     
	"CreatedOn" date,     
	"CompleteDate" date,     
	"StaffName" text,     
	"Building" text,    
	"Activities" text,    
	"Total" numeric DEFAULT 0.0 NOT NULL,     
	"Status" text,     
	"RulpaymentStatus" text,     
	"UploadId" text,     
	"CreatedDate" timestamp with time zone NOT NULL,     
	"CreatedBy" text,     
	"UpdatedDate" timestamp with time zone NOT NULL,     
	"UpdatedBy" text,     
	"ClaimedDate" date,     
	"ClaimedNumber" integer DEFAULT 0 NOT NULL 
) PARTITION BY RANGE ("CompleteDate");  

-- Step 3: Create the partitions for each year with adjusted upper boundary dates 
CREATE TABLE public."Claims_2022" PARTITION OF public."Claims" FOR VALUES FROM ('2022-01-01') TO ('2023-01-01');  

CREATE TABLE public."Claims_2023" PARTITION OF public."Claims" FOR VALUES FROM ('2023-01-01') TO ('2024-01-01');  

CREATE TABLE public."Claims_2024" PARTITION OF public."Claims" FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');  

CREATE TABLE public."Claims_2025" PARTITION OF public."Claims" FOR VALUES FROM ('2025-01-01') TO ('2026-01-01');  

-- Step 4: Transfer data from the old table to the new partitioned table 
INSERT INTO public."Claims" SELECT * FROM public."Claims_old";  

-- Step 5: Drop the old table if no longer needed 
DROP TABLE public."Claims_old";  

-- Step 6: Create indexes on the partitions as needed 
CREATE INDEX idx_claims_2022_orderid ON public."Claims_2022"("OrderId"); 
CREATE INDEX idx_claims_2022_username ON public."Claims_2022"("UserName"); 
CREATE INDEX idx_claims_2023_orderid ON public."Claims_2023"("OrderId"); 
CREATE INDEX idx_claims_2023_username ON public."Claims_2023"("UserName"); 
CREATE INDEX idx_claims_2024_orderid ON public."Claims_2024"("OrderId"); 
CREATE INDEX idx_claims_2024_username ON public."Claims_2024"("UserName"); 
CREATE INDEX idx_claims_2025_orderid ON public."Claims_2025"("OrderId"); 
CREATE INDEX idx_claims_2025_username ON public."Claims_2025"("UserName");
```


This setup ensures that each partition correctly includes all dates within the specified range, avoiding errors related to partition boundaries.