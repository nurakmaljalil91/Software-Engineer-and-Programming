## Overview

If the [[EF Core]] Tool is outdated, error will shown

```shell
$ dotnet ef migrations add AddProductsTable
Build started...
Build succeeded.
The Entity Framework tools version '7.0.9' is older than that of the runtime '8.0.6'. Update the tools for the latest features and bug fixes. See https://aka.ms/AAc1fbw for more information.
Unable to create a 'DbContext' of type ''. The exception 'Unable to resolve service for type 'Microsoft.EntityFrameworkCore.DbContextOptions`1[EFCoreMigrations.ApplicationDbContext]' while attempting to activate 'EFCoreMigrations.ApplicationDbContext'.' was thrown while attempting to create an instance. For the different patterns supported at design time, see https://go.microsoft.com/fwlink/?linkid=851728
```

## Update command

Run this command to update the [[EF Core]] Tool

```shell
$ dotnet tool update --global dotnet-ef
Tool 'dotnet-ef' was successfully updated from version '7.0.9' to version '8.0.6'.
```

## Read More

- [https://stackoverflow.com/questions/52108659/need-to-update-ef-core-tools](https://stackoverflow.com/questions/52108659/need-to-update-ef-core-tools)