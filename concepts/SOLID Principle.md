## Overview

Five Object-Oriented design guidelines that help develop software that's easier to maintain, understand and extend. 

### Single Responsibility Principle (SRP)

A class should have one reason to change or having one responsibility.

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


