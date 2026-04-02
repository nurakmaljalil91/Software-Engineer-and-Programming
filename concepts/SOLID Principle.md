---
title: SOLID Principle
category: concepts
tags:
  - concepts
created: 2026-03-28
updated: 2026-03-28
status: active
---
## Overview

Five Object-Oriented design guidelines that help develop software that's easier to maintain, understand and extend. 

### Single Responsibility Principle (SRP)

A class should have one reason to change or having **one responsibility**.

Bad example:

```c#
public class Invoice
{
	public void CalculateTotal() {}
	public void SaveToDatabase() {}	
}
```

Good example:

```c#
public class Invoice
{
	public void CalculateTotal() {}
}

public class InvoiceRepository
{
	public void SaveToDatabase(Invoice invoice) {}
}
```

### Open/Closed Principle (OCP)

Class should be **open for extension** but closed for modification

Bad example:

```csharp
public class DiscountService
{
	public double GetDiscount(string customerType)
	{
		if (customerType == "Regualar") return 0.1;
		if (customerType == "Premium") return 0.2; // Every new type required    changing this method
		return 0;
	}
}
```

Good example:

```csharp
public interface IDiscountStrategy
{
	double GetDiscount();
}

public class RegularDiscount : IDiscountStrategy
{
	public double GetDiscount() => 0.1;
}

public class PremiumDiscount : IDiscountStrategy
{
	public double GetDiscount() => 0.2;
}
```

### Liskov Substitution Principle (LSP)

Subclass must be substitutable for their base type **without altering correct behavior**

Bad Example:

```csharp
public class Bird
{
	public virtual void Fly() {}
}

public class Ostrich : Bird
{
	public override void Fly() { throw new NotSupportedException(); }
}
```

Good example:

```csharp
public abstract class Bird { }

public class FlyingBird : Bird
{
	public void Fly() { }
}

public class Ostrich : Bird { }
```

### Interface Segregation Principle (ISP)

No client should be force to depend on methods it does not use

Bad example:

```csharp
public interface IWorker
{
	 void Work();
	 void Eat();
}

public class Robot : IWorker
{
	public void Work() {}
	public void Eat() { throw new NotImplementedException(); }
}
```

Good example:

```csharp
public interface IWorkable { void Work(); }
public interface IEatable { void Eat(); }

public class Robot : IWorkerable
{
	public void Work() { }
}
```

### Dependency Inversion Principle (DIP)

High-level modules should not depend on low-level modules; both should depend on abstractions.

Bad example:

```csharp
public class ReportService
{
    private SqlReportRepository _repository = new SqlReportRepository(); // tightly coupled
}
```

Good example:

```csharp
public interface IReportRepository { /* ... */ }

public class ReportService
{
    private readonly IReportRepository _repository;
    public ReportService(IReportRepository repository)
    {
        _repository = repository;
    }
}
```

### Quick Summary Table

|Principle|Focus|Main Benefit|
|---|---|---|
|**S**ingle Responsibility|One job per class|Easier maintenance|
|**O**pen/Closed|Extend without modifying|Safer updates|
|**L**iskov Substitution|Subclasses honor base behavior|No unexpected bugs|
|**I**nterface Segregation|Small, focused interfaces|Avoids unused code|
|**D**ependency Inversion|Depend on abstractions|Loose coupling|