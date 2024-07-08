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