## Overview

Software design approach that promotes the separation of concerns, ensuring systems are maintainable, scalable, and testable.

## Anatomy

### Domain 

Entities
ValueObjects
Domain Events
Domain Services

### Application

Use Cases (Command/Queries)
Ports/Interfaces
Validators
DTOs
Handler (MediatR)
Transaction Orchestration


### Infrastructure

EF Core DbContext
Repositories
Message Bus (Kafka/RabbitMQ/Service Bus)
Outbox
External Clients (HTTP)

### Presentations

Minimal APIs
Controller
AuthN
Exception Mapping
Idempotency