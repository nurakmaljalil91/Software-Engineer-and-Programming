- Suddenly have issues to commit 
- When running 
```bash
git commit -m "Query claim using raw sql instead of EF Core"
```

- Get this output
```bash
error: invalid object 100644 21deb3cbadea5c04c7c5f0cf0e5c9062b9b4537f for 'src/Infrastructure/Persistence/Migrations/20230102140600_UpdateLoanMigration.Designer.cs'
error: invalid object 100644 21deb3cbadea5c04c7c5f0cf0e5c9062b9b4537f for 'src/Infrastructure/Persistence/Migrations/20230102140600_UpdateLoanMigration.Designer.cs'
error: Error building trees
```

- Solution is to generate back the missing hash object by running this command

```bash
git hash-object -w src/Infrastructure/Persistence/Migrations/20230102140600_UpdateLoanMigration.Designer.cs

git hash-object -w src/Infrastructure/Persistence/Migrations/20230102140600_UpdateLoanMigration.Designer.cs
```

- Read more on this [stackoverflow](https://stackoverflow.com/questions/14448326/git-commit-stopped-working-error-building-trees) answer
