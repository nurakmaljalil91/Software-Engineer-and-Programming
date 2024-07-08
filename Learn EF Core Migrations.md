## Overview

The migrations feature in [[EF Core]] provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.

## Creating and customizing migrations

### Create simple minimal API Project

Create a simple project with minimal API

```shell
dotnet new webapi -o EFCoreMigrations
cd EFCoreMigrations
```

### Add Required NuGet Packages

Add a NuGet package of  `Npgsql.EntityFrameworkCore.PostgreSQL`

```shell
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL
```

Add a NuGet package `Microsoft.EntityFrameworkCore.Design`.This package is required for the Entity Framework Core Tools to work.

```shell
dotnet add package Microsoft.EntityFrameworkCore.Design
```

### Build and run the Project

Trust the HTTPS development certificate by running the following command:

```shell
dotnet dev-certs https --trust
```

If dialog show up, click **Yes**

See [Trust the ASP.NET Core HTTPS development certificate](https://learn.microsoft.com/en-us/aspnet/core/security/enforcing-ssl?view=aspnetcore-8.0#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.

Run the following command to start the app on the `https` profile:

```shell
dotnet run --launch-profile https
```

### Create Application Db Context

Create a `ApplicationDbContext` class

```c#
using Microsoft.EntityFrameworkCore;

namespace EFCoreMigrations;

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options) { }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);
    }
}

```

### Create Model Class

Create a `Product` class inside `Models` folder

```c#
namespace EFCoreMigrations.Models;

public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
    public decimal Price { get; set; }
}
```

### Configure Application Db Context

Configure the `EF Core` entity by configuring it in `ApplicationDbContext` class

Create the entity table name and add constraint for the price not to be negative

```c#
builder.ToTable("Products", tableBuilder =>
{
    tableBuilder.HasCheckConstraint(
        "CK_Price_NotNegative",
        sql: $"{nameof(Product.Price)} > 0"
    );
});
```

Create a primary key by specifying the property. [[EF Core]] will automatically specify it if not specify

```c#
builder.HasKey(p => p.Id);
```

Specify other properties

```c#
builder.Property(p => p.Name).HasMaxLength(100);

builder.Property(p =>p.Description).HasMaxLength(1000);

builder.Property(p =>p.Price).HasPrecision(18, 2);
```

Create an index using the name

```c#
builder.HasIndex(p => p.Name).IsUnique();
```

Full code here:

```c#
using EFCoreMigrations.Models;
using Microsoft.EntityFrameworkCore;

namespace EFCoreMigrations;

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options) { }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>(builder =>
        {
            builder.ToTable("Products", tableBuilder =>
            {
                tableBuilder.HasCheckConstraint(
                    "CK_Price_NotNegative",
                    sql: $"{nameof(Product.Price)} > 0"
                    );
            });

            builder.HasKey(p => p.Id);

            builder.Property(p => p.Name).HasMaxLength(100);

            builder.Property(p =>p.Description).HasMaxLength(1000);

            builder.Property(p =>p.Price).HasPrecision(18, 2);

            builder.HasIndex(p => p.Name).IsUnique();
        });
    }
}
```

### Initialize Application Db Context to Program

Modify the `Program.cs` class to initialize `ApplicationDbContext`. Add the snippet code

```c#
var connectionString = builder.Configuration.GetConnectionString("DefaultConnection");

builder.Services.AddDbContext<ApplicationDbContext>(option => option.UseNpgsql(connectionString));
```

Full code:

```c#
// program.cs
using EFCoreMigrations;
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var connectionString = builder.Configuration.GetConnectionString("DefaultConnection");

builder.Services.AddDbContext<ApplicationDbContext>(option => option.UseNpgsql(connectionString));

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();

app.MapGet("/", () => "Hello World!");

app.Run();
```

Add `DefaultConnection` to the `appsettings.json`

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;port=5432;user id = postgres; password = somestrongpassword; database = efcore; pooling = true; Minimum Pool Size=0;Maximum Pool Size=100;"
  },
  "AllowedHosts": "*"
}
```
## Applying migrations

Run the following command to add the migrations

```shell
dotnet ef migrations add AddProductsTable
```

Migration folder will be created

```css
EFCoreMigrations
├── ApplicationDbContext.cs
├── appsettings.Development.json
├── appsettings.json
├── EFCoreMigrations.csproj
├── EFCoreMigrations.csproj.user
├── EFCoreMigrations.http
├── EFCoreMigrations.sln
├── Program.cs
└── Models
    └── Product.cs
└── Migrations
    └── 20240701134414_AddProductsTable.cs
    └── 20240701134414_AddProductsTable.Desginer.cs
    └── ApplicationDbContextModelSnapshot.cs
```

### Updating Database

To update the database

```shell
dotnet ef database update AddProductsTable
```

### View lists of Migrations

```shell
dotnet ef migrations list
```

## Applying Migration through Code

Migration can also being applied by manually add it in the code. Create a static function to apply the migration

```c#
static void ApplyMigration<TDbContext>(IServiceScope scope)
    where TDbContext : DbContext
{
    using TDbContext context = scope.ServiceProvider
        .GetRequiredService<TDbContext>();

    context.Database.Migrate();
}
```

Add it in the `Program.cs` where application environment is in development.

```c#
// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    using IServiceScope scope = app.Services.CreateScope();
    ApplyMigration<ApplicationDbContext>(scope);
}
```

Full code:

```c#
using EFCoreMigrations;
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var connectionString = builder.Configuration.GetConnectionString("DefaultConnection");

builder.Services.AddDbContext<ApplicationDbContext>(option => option.UseNpgsql(connectionString));

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    using IServiceScope scope = app.Services.CreateScope();

    ApplyMigration<ApplicationDbContext>(scope);

    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();

app.MapGet("/", () => "Hello World!");

app.Run();

static void ApplyMigration<TDbContext>(IServiceScope scope)
    where TDbContext : DbContext
{
    using TDbContext context = scope.ServiceProvider
        .GetRequiredService<TDbContext>();

    context.Database.Migrate();
}
```
## SQL Scripts

Generating SQL Script is much recommended in production where you can't apply the migration. The following generates a SQL script from a blank database to the latest migration:

```shell
dotnet ef migrations script
```
### With From (to implied)

The following generates a SQL script from the given migration to the latest migration.

```shell
dotnet ef migrations script AddNewTables
```
### With From and To

The following generates a SQL script from the specified `from` migration to the specified `to` migration.

```shell
dotnet ef migrations script AddNewTables AddAuditTable
```

You can use a `from` that is newer than the `to` in order to generate a rollback script.

> Warning!!
> Please take note of potential data loss scenarios.

## Database Migration Tools