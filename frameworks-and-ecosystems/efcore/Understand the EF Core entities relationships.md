---
title: Understand the EF Core entities relationships
category: efcore
tags:
  - efcore
created: 2026-03-28
updated: 2026-03-28
status: active
---
## EF Core Primary and Foreign Key

- By default if you have `Id`  or `<typeName>Id` in your entity it will become primary key
```c#
internal class Car
{
    public string Id { get; set; } // this will become primary key

    public string Make { get; set; }
    public string Model { get; set; }
}

internal class Truck
{
    public string TruckId { get; set; } // this will become primary key

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## Relationships

### Required one-to-many