## Database Migrations  

To use `dotnet-ef` for your migrations first ensure that `UseInMemoryDatabase` is disabled, as described within previous section. Then, add the following flags to your command (values assume you are executing from repository root)  
  
- `--project src/Infrastructure (optional if in this folder)`  
- `--startup-project src/WebAPI`  
- `--output-dir Persistence/Migrations`  
  
For example, to add a new migration from the root folder:  

```bash  
dotnet ef migrations add "MainMigration" --project src\Infrastructure --startup-project src\WebAPI --output-dir Persistence\Migrations
```  

If you get this message error

```bash
C:\Users\User\Developments\PKTBM\API>dotnet ef migrations add "FirstMigration" --project src\Infrastructure --startup-pr
oject src\WebAPI --output-dir Persistence\Migrations
Build started...
Build succeeded.
Your startup project 'WebAPI' doesn't reference Microsoft.EntityFrameworkCore.Design. This package is required for the Entity Framework Core Tools to work. Ensure your startup project is correct, install the package, and try again.
```

Install `Microsoft.EntityFrameworkCore.Design` using nuget package in the web API project

To update the database:  

```bash  
dotnet ef database update "MainMigration" --project src\Infrastructure --startup-project src\WebAPI
```

To see all the list of the migrations

```bash
dotnet ef migrations list --project src\Infrastructure --startup-project src\WebAPI
```
