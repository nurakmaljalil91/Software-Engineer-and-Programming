---
title: Clean Architecture
category: architectures
tags: [#architectures]
created: 2026-03-28
updated: 2026-03-28
status: active
---
## Overview

Software design approach that promotes the separation of concerns, ensuring systems are maintainable, scalable, and testable. Clean architecture is a layered approach to structuring software so that business rules are insulated from details like databases and web frameworks.  It promotes separation of concerns and helps keep systems maintainable, scalable and testable

## Anatomy

### Domain 

#### Entities & Value Objects
 
 The core business types.  Entities have an identity and can change over time, while value objects are immutable and defined only by their values
#### Domain Events

Messages used to model significant occurrences inside the domain, enabling other parts of the system to react asynchronously.
#### Domain Services

Stateless operations that don’t naturally belong on a single entity; they encapsulate domain logic

The domain layer contains **no** direct dependencies on frameworks or infrastructure code.

```c#
// Value Object
public readonly record struct Money(decimal Amount, string Currency)
{
    public static Money Zero(string currency) => new(0m, currency);
    public Money EnsureCurrency(string currency) =>
        Currency == currency ? this : throw new InvalidOperationException("Currency mismatch.");
    public static Money operator +(Money a, Money b)
        => a.Currency == b.Currency ? new(a.Amount + b.Amount, a.Currency)
                                    : throw new InvalidOperationException("Currency mismatch.");
    public override string ToString() => $"{Currency} {Amount:N2}";
}

// Strongly-typed IDs (simple approach)
public readonly record struct OrderId(Guid Value)
{
    public static OrderId New() => new(Guid.NewGuid());
    public override string ToString() => Value.ToString();
}
public readonly record struct ProductId(Guid Value);

// Domain Event marker
public interface IDomainEvent { }

// A specific domain event
public sealed record OrderPlaced(OrderId OrderId, Money Total) : IDomainEvent;

// Aggregate root
public sealed class Order
{
    private readonly List<OrderItem> _items = new();
    private readonly List<IDomainEvent> _domainEvents = new();

    public OrderId Id { get; private set; }
    public Guid CustomerId { get; private set; } // could also be a strong ID
    public string Currency { get; private set; } = "MYR";
    public OrderStatus Status { get; private set; } = OrderStatus.Draft;
    public IReadOnlyCollection<OrderItem> Items => _items.AsReadOnly();
    public IReadOnlyCollection<IDomainEvent> DomainEvents => _domainEvents.AsReadOnly();

    private Order() { } // EF
    public Order(Guid customerId, string currency = "MYR")
    {
        Id = OrderId.New();
        CustomerId = customerId;
        Currency = currency;
    }

    public void AddItem(ProductId productId, int quantity, Money unitPrice)
    {
        if (quantity <= 0) throw new ArgumentOutOfRangeException(nameof(quantity));
        unitPrice = unitPrice.EnsureCurrency(Currency);

        var existing = _items.FirstOrDefault(i => i.ProductId == productId);
        if (existing is null)
            _items.Add(new OrderItem(productId, quantity, unitPrice));
        else
            existing.Increase(quantity);
    }

    public void RemoveItem(ProductId productId)
    {
        var item = _items.FirstOrDefault(i => i.ProductId == productId)
                   ?? throw new InvalidOperationException("Item not found.");
        _items.Remove(item);
    }

    public Money Total() => _items.Aggregate(Money.Zero(Currency), (acc, i) => acc + i.Total());

    public void Place()
    {
        if (!_items.Any()) throw new InvalidOperationException("Cannot place empty order.");
        if (Status != OrderStatus.Draft) throw new InvalidOperationException("Order already placed.");
        Status = OrderStatus.Placed;
        _domainEvents.Add(new OrderPlaced(Id, Total()));
    }

    public void ClearDomainEvents() => _domainEvents.Clear();
}

public enum OrderStatus { Draft = 0, Placed = 1, Cancelled = 2 }

// Entity within the aggregate
public sealed class OrderItem
{
    public int Id { get; private set; } // EF key inside the aggregate table (not exposed)
    public ProductId ProductId { get; private set; }
    public int Quantity { get; private set; }
    public Money UnitPrice { get; private set; }

    private OrderItem() { } // EF
    public OrderItem(ProductId productId, int quantity, Money unitPrice)
    {
        if (quantity <= 0) throw new ArgumentOutOfRangeException(nameof(quantity));
        ProductId = productId;
        Quantity = quantity;
        UnitPrice = unitPrice;
    }

    public void Increase(int quantity)
    {
        if (quantity <= 0) throw new ArgumentOutOfRangeException(nameof(quantity));
        Quantity += quantity;
    }

    public Money Total() => new(UnitPrice.Amount * Quantity, UnitPrice.Currency);
}

// Repositories (defined in Domain or Application)
public interface IOrderRepository
{
    Task<Order?> GetByIdAsync(OrderId id, CancellationToken ct = default);
    Task AddAsync(Order order, CancellationToken ct = default);
    void Remove(Order order);
}

public interface IUnitOfWork
{
    Task<int> SaveChangesAsync(CancellationToken ct = default);
}

```
## Application layer

#### Use cases 
Often implemented as commands and queries that encapsulate a single business operation .In .NET you can model these using MediatR handlers or application services.

#### Ports/interfaces
Contracts that define how the application communicates with external resources such as repositories or messaging systems.

#### DTOs and validators

Define request/response shapes and enforce business rules.
#### Transaction orchestration

Ensures business operations execute atomically.

The application layer depends on the domain but not on infrastructure.

```c#
public sealed class PlaceOrderRequest
{
    public Guid CustomerId { get; init; }
    public string Currency { get; init; } = "MYR";
    public List<PlaceOrderItem> Items { get; init; } = new();
}
public sealed class PlaceOrderItem
{
    public Guid ProductId { get; init; }
    public int Quantity { get; init; }
    public decimal UnitPrice { get; init; }
}

public sealed class PlaceOrderResponse
{
    public Guid OrderId { get; init; }
    public decimal Total { get; init; }
    public string Currency { get; init; } = "MYR";
}

public sealed class OrderAppService(IOrderRepository repo, IUnitOfWork uow)
{
    public async Task<PlaceOrderResponse> PlaceOrderAsync(PlaceOrderRequest request, CancellationToken ct = default)
    {
        var order = new Order(request.CustomerId, request.Currency);

        foreach (var i in request.Items)
        {
            order.AddItem(new ProductId(i.ProductId), i.Quantity, new Money(i.UnitPrice, request.Currency));
        }

        order.Place(); // raises OrderPlaced event

        await repo.AddAsync(order, ct);
        await uow.SaveChangesAsync(ct); // infra will persist & publish domain events

        var total = order.Total();
        return new PlaceOrderResponse
        {
            OrderId = order.Id.Value,
            Total = total.Amount,
            Currency = total.Currency
        };
    }
}
```
## Infrastructure layer

This layer implements the interfaces defined in the application or domain layer. Examples include:
#### [[EF Core]] DbContext and repositories

Classes that persist and retrieve aggregates.
#### Message brokers

Kafka, RabbitMQ or Azure Service Bus.
### Outbox pattern 
Ensures reliable message publication in the same transaction as database updates.
### External clients

HTTP clients to call downstream services.


```c#
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata.Builders;

// DbContext acts as Unit of Work
public sealed class SalesDbContext(DbContextOptions<SalesDbContext> options,
                                   IDomainEventDispatcher dispatcher)
    : DbContext(options), IUnitOfWork
{
    public DbSet<Order> Orders => Set<Order>();

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.ApplyConfiguration(new OrderConfig());
    }

    public override async Task<int> SaveChangesAsync(CancellationToken ct = default)
    {
        // collect domain events BEFORE saving (or after, depending on strategy)
        var domainEvents = ChangeTracker.Entries<Order>()
            .SelectMany(e => e.Entity.DomainEvents)
            .ToList();

        var result = await base.SaveChangesAsync(ct);

        // publish after commit (outbox recommended in real systems)
        await dispatcher.DispatchAsync(domainEvents, ct);

        foreach (var entry in ChangeTracker.Entries<Order>())
            entry.Entity.ClearDomainEvents();

        return result;
    }
}

// EF type configuration for the aggregate root
file sealed class OrderConfig : IEntityTypeConfiguration<Order>
{
    public void Configure(EntityTypeBuilder<Order> b)
    {
        b.ToTable("Orders");
        b.HasKey(o => o.Id);
        b.Property(o => o.Id)
            .HasConversion(
                id => id.Value,
                guid => new OrderId(guid))
            .ValueGeneratedNever();

        b.Property(o => o.CustomerId).IsRequired();
        b.Property(o => o.Currency).HasMaxLength(3).IsRequired();
        b.Property(o => o.Status).HasConversion<int>().IsRequired();

        // Items as owned collection (stays inside aggregate boundary)
        b.OwnsMany(o => o.Items, items =>
        {
            items.ToTable("OrderItems");
            items.WithOwner().HasForeignKey("OrderId");
            items.HasKey("Id"); // EF-generated key

            items.Property(i => i.ProductId)
                 .HasConversion(
                    pid => pid.Value,
                    guid => new ProductId(guid))
                 .IsRequired();

            items.Property(i => i.Quantity).IsRequired();

            // Map Money as owned object inside OrderItem
            items.OwnsOne(i => i.UnitPrice, money =>
            {
                money.Property(m => m.Amount).HasColumnName("UnitPriceAmount").HasPrecision(18, 2);
                money.Property(m => m.Currency).HasColumnName("UnitPriceCurrency").HasMaxLength(3);
            });
        });
    }
}

// Repository
public sealed class OrderRepository(SalesDbContext db) : IOrderRepository
{
    public async Task<Order?> GetByIdAsync(OrderId id, CancellationToken ct = default)
        => await db.Orders
            .Include(o => o.Items)
            .FirstOrDefaultAsync(o => o.Id == id, ct);

    public async Task AddAsync(Order order, CancellationToken ct = default)
        => await db.Orders.AddAsync(order, ct);

    public void Remove(Order order) => db.Orders.Remove(order);
}

// Domain event dispatch (simple; replace with MediatR or Outbox in prod)
public interface IDomainEventDispatcher
{
    Task DispatchAsync(IEnumerable<IDomainEvent> events, CancellationToken ct = default);
}

public sealed class DomainEventDispatcher(IServiceProvider sp) : IDomainEventDispatcher
{
    public async Task DispatchAsync(IEnumerable<IDomainEvent> events, CancellationToken ct = default)
    {
        // naïve: resolve handlers dynamically
        foreach (var @event in events)
        {
            var handlerType = typeof(IDomainEventHandler<>).MakeGenericType(@event.GetType());
            var handlers = (IEnumerable<object>)sp.GetService(typeof(IEnumerable<>).MakeGenericType(handlerType)) 
                           ?? Enumerable.Empty<object>();

            foreach (var handler in handlers)
            {
                var method = handlerType.GetMethod(nameof(IDomainEventHandler<IDomainEvent>.HandleAsync))!;
                var task = (Task)method.Invoke(handler, new[] { @event, ct })!;
                await task.ConfigureAwait(false);
            }
        }
    }
}

public interface IDomainEventHandler<in TEvent> where TEvent : IDomainEvent
{
    Task HandleAsync(TEvent e, CancellationToken ct);
}

// Example handler
public sealed class OrderPlacedEmailHandler : IDomainEventHandler<OrderPlaced>
{
    public Task HandleAsync(OrderPlaced e, CancellationToken ct)
    {
        // send email, enqueue message, etc. (side-effect)
        Console.WriteLine($"Order {e.OrderId} placed. Total: {e.Total}");
        return Task.CompletedTask;
    }
}
```

## Presentation layer

The outermost layer delivers the application to users or other systems.  In .NET this could be an ASP.NET Core minimal API, MVC controller or gRPC service. Responsibilities include:
#### Endpoint routing
Mapping HTTP requests to handlers (e.g., minimal APIs or controllers).
#### Authentication and authorization

(AuthN/AuthZ).
### Exception mapping and idempotency 
Translating domain exceptions to HTTP responses and ensuring idempotent operations.

```C#
// Program.cs
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddDbContext<SalesDbContext>(opt =>
    opt.UseNpgsql(builder.Configuration.GetConnectionString("Sales"))); // or SqlServer

builder.Services.AddScoped<IOrderRepository, OrderRepository>();
builder.Services.AddScoped<IUnitOfWork>(sp => sp.GetRequiredService<SalesDbContext>());
builder.Services.AddScoped<IDomainEventDispatcher, DomainEventDispatcher>();
builder.Services.Scan(scan => scan // register all domain event handlers in assembly
    .FromAssemblyOf<OrderPlacedEmailHandler>()
    .AddClasses(classes => classes.AssignableTo(typeof(IDomainEventHandler<>)))
    .AsImplementedInterfaces()
    .WithScopedLifetime());

builder.Services.AddScoped<OrderAppService>();

var app = builder.Build();
app.MapPost("/orders", async (PlaceOrderRequest req, OrderAppService svc, CancellationToken ct) =>
{
    var result = await svc.PlaceOrderAsync(req, ct);
    return Results.Created($"/orders/{result.OrderId}", result);
});

app.Run();
```
## Key principles

**Dependency rule** – source code dependencies always point inward; outer layers depend on inner layers, never the other way around.  For example, domain types do not reference infrastructure classes.

**Inversion of control** – outer layers implement interfaces defined in inner layers, making it easy to swap implementations or perform unit testing.

**Independent deployment** – by isolating business logic, changes to the UI or persistence layer have minimal impact on the domain.

Adopting clean architecture in .NET encourages testability, maintainability and clear separation between business logic and infrastructure.