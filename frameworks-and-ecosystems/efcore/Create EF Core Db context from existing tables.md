---
title: Create EF Core Db context from existing tables
category: efcore
tags:
  - efcore
created: 2026-03-28
updated: 2026-03-28
status: active
---
## Scaffold Db Context

- Create a code from table 

```bash
Scaffold-DbContext "Server=127.0.0.1;port=5432;user id = postgres; password = admin4321; 
database = ruldb; pooling = true; Minimum Pool Size=0;Maximum Pool Size=100;" Npgsql.EntityFrameworkCore.PostgreSQL -OutputDir Persistence -context ApplicationDbContext
```

- Specify the table or if want to create new table

```bash
Scaffold-DbContext "Server=127.0.0.1;port=5432;user id = postgres; password = admin4321; 
database = ruldb; pooling = true; Minimum Pool Size=0;Maximum Pool Size=100;" Npgsql.EntityFrameworkCore.PostgreSQL -OutputDir Persistence -context ApplicationDbContext -Tables Refund,RefundTransaction
```