---
title: Using Transaction in Postgresql
category: postgresql
tags:
  - postgresql
created: 2026-03-28
updated: 2026-05-01
status: active
---

## What is a Transaction

A **transaction** is a group of operations that behave as a single unit:

- **BEGIN** → start
- **COMMIT** → save changes
- **ROLLBACK** → cancel everything

Guarantees **ACID**: atomicity, consistency, isolation, durability.

## Examples

```sql
SELECT * FROM public."Claims" WHERE "OrderId" = '2509000082673862';

BEGIN;

UPDATE public."Claims"
SET "RulpaymentStatus" = 'To Review'
WHERE "OrderId" = '2509000082673862';

SAVEPOINT savepoint1;

SELECT * FROM public."Claims" WHERE "OrderId" = '2509000082673862';

ROLLBACK TO savepoint1;

COMMIT;
```

- Using variable

```sql
-- 1. Define the variable for this session
SET session my.vars.order_id = '1-123282263993';

-- 2. Use it in your SELECT
SELECT * FROM public."Claims" 
WHERE "OrderId" = current_setting('my.vars.order_id');

BEGIN;

-- 3. Use it in your UPDATE
UPDATE public."Claims"
SET "RulpaymentStatus" = 'To Review'
WHERE "OrderId" = current_setting('my.vars.order_id');

SAVEPOINT savepoint1;

-- 4. Verify again
SELECT * FROM public."Claims" 
WHERE "OrderId" = current_setting('my.vars.order_id');

-- ROLLBACK TO savepoint1;
-- COMMIT;```

## Using Transaction in .NET (Npgsql)

```csharp
await using var conn = new NpgsqlConnection(connectionString);
await conn.OpenAsync();

await using var transaction = await conn.BeginTransactionAsync();

try
{
    await using (var cmd = new NpgsqlCommand("UPDATE accounts SET balance = balance - 100 WHERE id = 1", conn, transaction))
        await cmd.ExecuteNonQueryAsync();

    await using (var cmd = new NpgsqlCommand("UPDATE accounts SET balance = balance + 100 WHERE id = 2", conn, transaction))
        await cmd.ExecuteNonQueryAsync();

    await transaction.CommitAsync(); // save
}
catch
{
    await transaction.RollbackAsync(); // undo
    throw;
}
```

## Using Transaction in EFCore

```csharp
using var transaction = await context.Database.BeginTransactionAsync();

try
{
    var loan = new Loan { ... };
    context.Loans.Add(loan);
    await context.SaveChangesAsync();

    var tx = new LoanTransaction { LoanId = loan.Id, Amount = 100 };
    context.LoanTransactions.Add(tx);
    await context.SaveChangesAsync();

    await transaction.CommitAsync();
}
catch
{
    await transaction.RollbackAsync();
    throw;
}
```