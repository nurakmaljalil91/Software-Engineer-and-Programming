---
title: Domain Driven Design
category: architectures
tags: [#architectures]
created: 2026-03-28
updated: 2026-03-28
status: active
---
Domain‑Driven Design is an approach to building software that focuses on understanding and modeling the problem domain.  The goal is to close the gap between business experts and developers by using a shared **ubiquitous language** and organizing code around meaningful domain concepts rather than technical concerns.
## Core building blocks
### Entities

- Represent concepts with a distinct identity that persists over time (e.g., `Order`, `Customer`).
- Have mutable state and are compared by identity rather than by their attributes.
- In a [[Clean Architecture]] these entities live in the **domain layer**.
### Value objects

- Describe attributes or measurements (e.g., `Money`, `DateRange`, `Address`) and are immutable.
- They don’t have identity; two value objects are equal if all their properties are equal.
- They help keep entities small and focused by extracting descriptive data into separate types.

```cs
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
```
### [[Aggregates and Aggregate Roots]]

- An **aggregate** is a cluster of entities and value objects that are treated as a unit for data changes.

- The **aggregate root** is the single entry point to an aggregate; external code never references internal entities directly.  This ensures invariants are enforced.

```cs
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
### Domain services

- Represent domain operations that don’t naturally belong to a single entity or value object (e.g., calculating the best route between two cities).
- Should be stateless and pure; they orchestrate calls to entities and value objects.
### Repositories

- Abstract away the details of data access.  A repository returns and persists aggregates.
- They live in the infrastructure layer but implement interfaces defined in the domain or application layer.
### Domain events

- Capture significant occurrences in the domain (e.g., `OrderPlaced`).
- They enable loosely coupled communication between aggregates and application services.
### Factories

- Encapsulate complex creation logic for aggregates, ensuring invariants are satisfied at construction time.
### Bounded context

- A boundary within which a specific domain model and ubiquitous language apply.
- Different sub‑domains may use the same terms to mean different things; bounded contexts keep models cohesive and explicit.
## Modelling process

1. **Collaborate with domain experts** – build a ubiquitous language by talking to business stakeholders.  Capture terminology in code via class and method names.
2. **Identify sub‑domains and bounded contexts** – break the problem space into cohesive areas with clear boundaries.
3. **Create aggregates** – design entities and value objects with proper invariants and choose aggregate roots.
4. **Define domain services and events** – model operations and behaviors that cross aggregate boundaries.
5. **Implement repositories** – provide persistence mechanisms via interfaces; infrastructure classes implement these interfaces.
6. **Iterate** – evolve the model as your understanding of the domain deepens.

By aligning code with the business language and enforcing clear boundaries, DDD helps create systems that are easier to reason about and evolve.