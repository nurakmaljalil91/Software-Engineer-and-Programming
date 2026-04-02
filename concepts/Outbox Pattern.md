---
title: Outbox Pattern
category: concepts
tags:
  - concepts
created: 2026-03-28
updated: 2026-03-28
status: active
---
This is one of the most important patterns to learn for building reliable distributed systems in .NET.
The **Outbox Pattern** solves a very specific, common problem called the "Dual-Write Problem."
Here is a comprehensive guide on what it is, why you need it, and how to implement it in C\#.

-----
## The Problem: "Dual-Write"

Imagine you have an e-commerce system. When a user places an order, you need to do two things:

1.  **Save** the order to your database (SQL Server/PostgreSQL).
2.  **Publish** an event (e.g., `OrderCreated`) to a Message Broker (RabbitMQ, Kafka, [[Azure Service Bus]]) so the Shipping Service knows about it.

**The Danger:** These two systems (Database and Message Broker) cannot automatically share a transaction.

  * **Scenario A:** You save to the DB, but the Message Broker is down. The event is lost. The Shipping Service never knows to ship the item.
  * **Scenario B:** You publish the event, but the DB save fails (constraint violation). Now Shipping tries to ship an order that doesn't exist.

### 2\. The Solution: Transactional Outbox

The Outbox Pattern ensures that **both** happen, or **neither** happens.

Instead of sending the message directly to the broker, you save the message into a table in your database (the "Outbox" table) inside the **same transaction** as your business data.

**The Flow:**

1.  Start Database Transaction.
2.  Insert Order into `Orders` table.
3.  Insert Event Payload (JSON) into `Outbox` table.
4.  Commit Transaction. (If this fails, everything rolls back).
5.  **The Relay:** A separate background process checks the `Outbox` table, picks up messages, and sends them to the Broker.

-----

### 3\. Implementing in .NET C\# (The Mechanics)

Here is how you would implement the core mechanics using EF Core.

#### Step A: The Outbox Entity

First, define what an outbox message looks like.

```csharp
public class OutboxMessage
{
    public Guid Id { get; set; }
    public string Type { get; set; } // e.g., "OrderCreated"
    public string Content { get; set; } // The JSON payload
    public DateTime OccurredOn { get; set; }
    public DateTime? ProcessedOn { get; set; }
    public string Error { get; set; }
}
```

#### Step B: Saving the Data (The Transaction)

In your Service or Controller, you save the business entity and the message together.

```csharp
public async Task PlaceOrder(OrderDto orderDto)
{
    using var transaction = _dbContext.Database.BeginTransaction();

    try
    {
        // 1. Save the Business Data
        var order = new Order { /* map properties */ };
        _dbContext.Orders.Add(order);

        // 2. Create the Event
        var orderCreatedEvent = new OrderCreatedEvent { OrderId = order.Id };
        string jsonPayload = JsonSerializer.Serialize(orderCreatedEvent);

        // 3. Save to Outbox (Same Context!)
        var outboxMessage = new OutboxMessage
        {
            Id = Guid.NewGuid(),
            Type = nameof(OrderCreatedEvent),
            Content = jsonPayload,
            OccurredOn = DateTime.UtcNow
        };
        _dbContext.OutboxMessages.Add(outboxMessage);

        // 4. Save and Commit
        await _dbContext.SaveChangesAsync();
        await transaction.CommitAsync();
    }
    catch
    {
        // If anything fails, neither the Order nor the Message exists in the DB.
        await transaction.RollbackAsync();
        throw;
    }
}
```

#### Step C: The Relay (The Background Worker)

You need a process to read the table and send the messages. In .NET, this is usually a `BackgroundService` (Hosted Service).

*Note: This is a simplified polling example. In production, you want to handle concurrency so multiple instances of your app don't send the same message.*

```csharp
public class OutboxProcessor : BackgroundService
{
    private readonly IServiceProvider _serviceProvider;

    public OutboxProcessor(IServiceProvider serviceProvider)
    {
        _serviceProvider = serviceProvider;
    }

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        while (!stoppingToken.IsCancellationRequested)
        {
            // Create a scope to get the DbContext
            using (var scope = _serviceProvider.CreateScope())
            {
                var dbContext = scope.ServiceProvider.GetRequiredService<AppDbContext>();
                var publisher = scope.ServiceProvider.GetRequiredService<IMessagePublisher>();

                // 1. Fetch unprocessed messages
                var messages = await dbContext.OutboxMessages
                    .Where(m => m.ProcessedOn == null)
                    .Take(20)
                    .ToListAsync(stoppingToken);

                foreach (var msg in messages)
                {
                    try
                    {
                        // 2. Send to Broker (RabbitMQ/AzureServiceBus)
                        await publisher.PublishAsync(msg.Type, msg.Content);

                        // 3. Mark as processed
                        msg.ProcessedOn = DateTime.UtcNow;
                    }
                    catch (Exception ex)
                    {
                        msg.Error = ex.Message;
                    }
                }
                
                // 4. Update DB to show messages are sent
                await dbContext.SaveChangesAsync(stoppingToken);
            }

            // Wait a bit before polling again
            await Task.Delay(TimeSpan.FromSeconds(2), stoppingToken);
        }
    }
}
```

-----

### 4\. The "Easy Button": Use Libraries

While understanding the code above is crucial, writing your own Outbox processor is risky (handling retries, concurrency, and locking is hard).

In the .NET ecosystem, we typically use libraries that handle the Outbox Pattern for us automatically.

#### 1\. MassTransit (Highly Recommended)

MassTransit is the most popular .NET library for messaging. It has a built-in `UseEntityFrameworkOutbox`.

**How it looks:**

```csharp
// Program.cs setup
services.AddMassTransit(x =>
{
    x.AddEntityFrameworkOutbox<AppDbContext>(o =>
    {
        o.UseSqlServer();
        o.UseBusOutbox(); // Moves messages to the outbox automatically
    });
    
    // ... Configure RabbitMQ/ServiceBus
});
```

**Usage:**
When you call `IPublishEndpoint.Publish()` inside your code, MassTransit **intercepts** it. It doesn't send it to RabbitMQ immediately. It detects you are in an EF Core transaction and saves it to a table it creates automatically. It then handles the background sending for you.

#### 2\. CAP (C\# AP)

CAP is a library specifically designed for the Outbox pattern ("Eventual Consistency"). It works great if you want something lighter than MassTransit.

-----

### 5\. Important Considerations

  * **Idempotency:** The Outbox pattern guarantees "At-Least-Once" delivery. If the Relay sends the message to RabbitMQ, but crashes *before* it can update the `Outbox` table to say "Processed," it will restart and send the message again.
      * *The Fix:* Your consumers (listeners) must be able to handle receiving the same message ID twice without breaking data.
  * **Order:** If strict ordering of messages is required (e.g., "Order Created" must be processed before "Order Updated"), you must ensure your Relay processes messages sequentially, which limits scalability.
  * **Cleanup:** The Outbox table will grow indefinitely. You need a scheduled job to delete rows where `ProcessedOn != null` and is older than X days.

### Summary

| Feature | Description |
| :--- | :--- |
| **Goal** | Ensure data consistency between Database and Message Broker. |
| **Mechanism** | Save the message to a DB table in the same transaction as business data. |
| **Relay** | A background process moves data from the DB table to the Message Broker. |
| **Best Practice** | Don't write your own poller; use **MassTransit** or **NServiceBus**. |
