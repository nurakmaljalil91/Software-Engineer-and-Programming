## Overview

PostgreSQL, also known as Postgres, is a free and open-source relational database management system emphasizing extensibility and SQL compliance.

Download PostgreSQL from [https://www.postgresql.org/download](https://www.postgresql.org/download)

## Backup data from Remote Database server to local file

```bash
pg_dump -U postgres -h 159.65.130.195 -p 5432 ruldb > backup.sql
```

- In [[Windows]]

```powershell
"C:\Program Files\PostgreSQL\16\bin\pg_dump" -U postgres -h 159.65.130.195 -p 5432 ruldb > ruldb-backup-2023-09-04.sql
```

- Enter the database user password
## Restore backup data to local Database server

```bash
psql -U postgres ruldb < backup.sql
```

- In [[Windows]]

```powershell
"C:\Program Files\PostgreSQL\16\bin\psql" -U postgres ruldb < ruldb-backup-2023-09-04.sql
```

- Enter the database user password

> Warning ! Don't use [[PowerShell]] to do this, just do [[CMD]]

## Temps

```sql
SELECT * FROM "Claims" WHERE "UserName" = 'Q102992';
SELECT * FROM "Claims" WHERE "UserName" = 'Q004551';
SELECT * FROM "Claims" WHERE "UserName" = 'Q104699';

UPDATE "Claims" SET "ClaimedNumber" = 1 WHERE "UserName" = 'Q102992' AND "RulpaymentStatus" = 'Paid' AND "ClaimedDate" = '2023-04-18'
UPDATE "Claims" SET "ClaimedNumber" = 2 WHERE "UserName" = 'Q102992' AND "RulpaymentStatus" = 'Paid' AND "ClaimedDate" = '2023-05-01'
```

## Find not exist in another table

```sql
SELECT * 
FROM public."AspNetUsers"
WHERE "UserName" NOT IN
	(SELECT "UserName" 
	FROM public."Staffs")
```

## Move data from one table to another table

```sql
WITH moved_rows AS (  
    DELETE FROM public."Thrashs" a  
    USING public."Claims" b  
    WHERE a."OrderId"='1-68813012600'  
    RETURNING a.* -- or specify columns  
)  
INSERT INTO public."Claims" --specify columns if necessary  
SELECT DISTINCT * FROM moved_rows;
```

## Backup data

- [[Copy all data from tables to csv]]
### Optimize Data

- [[Optimize the Query Performances]]
- [[Create Partition Table]] 

