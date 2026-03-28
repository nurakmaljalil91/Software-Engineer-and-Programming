---
title: Dependency Injection in DotNET
category: csharp
tags: [#csharp]
created: 2026-03-28
updated: 2026-03-28
status: active
---
**Dependency Injection (DI)** is a pattern that promotes loose coupling by removing the responsibility of constructing dependencies from classes.  Instead, required services are provided (“injected”) from the outside, typically by a framework‑managed container.  DI makes it easier to test and maintain your code.
## Inversion of control

- Traditional code often uses **new** to create objects directly.  This hard‑codes dependencies and makes classes difficult to test.
- With DI, your classes depend on **abstractions** (interfaces), and a container resolves and supplies concrete implementations.
- In ASP.NET Core, the built‑in IoC container lives inside the `Microsoft.Extensions.DependencyInjection` namespace.
## Registering services

Registration maps an abstraction to its implementation and defines the lifetime.  In .NET 6 and newer the registration usually happens in `Program.cs`:

```csharp
var builder = WebApplication.CreateBuilder(args);

// register services
builder.Services.AddTransient<IUserRepository, UserRepository>();   // new instance per resolve
builder.Services.AddScoped<IEmailService, EmailService>();           // one per HTTP request
builder.Services.AddSingleton<IClock, SystemClock>();             
// one per application
var app = builder.Build();
```

Common lifetimes:

- **Transient** – a new instance is created each time the service is requested.
- **Scoped** – one instance per scope (in web apps the scope is usually an HTTP request).
- **Singleton** – one instance for the lifetime of the application; shared by all consumers.
## Consuming services

### Constructor injection

The preferred pattern is to request dependencies via the constructor:

```csharp
public class OrderService
{
    private readonly IOrderRepository _repo;
    private readonly INotificationService _notifications;

    public OrderService(IOrderRepository repo, INotificationService notifications)
    {
        _repo = repo;
        _notifications = notifications;
    }

    public async Task PlaceOrderAsync(Order order)
    {
        await _repo.AddAsync(order);
        await _notifications.SendOrderConfirmationAsync(order);
    }
}
```

ASP.NET Core will automatically resolve and inject `IOrderRepository` and `INotificationService` from the container.
### Method and property injection

Less common forms allow dependencies to be supplied via setter properties or method parameters.  Use them sparingly; constructor injection provides clear requirements and immutability.
## Testing with DI

Because dependencies are injected via interfaces, you can replace concrete implementations with mocks or fakes during unit testing.  For example, using Moq:

```csharp
var repoMock = new Mock<IOrderRepository>();

var notificationsMock = new Mock<INotificationService>();

var service = new OrderService(repoMock.Object, notificationsMock.Object);
```

  DI helps keep your application modular, testable and adaptable.  Combined with clean architecture and DDD, it ensures that higher‑level policies are not tightly coupled to low‑level implementation details.