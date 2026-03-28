---
title: Auto-Property in CSharp
category: csharp
tags: [#csharp]
created: 2026-03-28
updated: 2026-03-28
status: active
---

## Overview

Auto-implementation property (or "auto-property") will declare a property without explicitly writing a backing field.

Compiler generate the hidden field.

Reduce boilerplate (make simple get/set access).

## Basic syntax

```c#
public class Person
{
	// Auto-implemented property
	public string Name { get; set; }
}

// Usage
var person = new Person();
person.Name = "Sakura";
Console.WriteLine(person.Name); // "Sakura"
```

Here the compiler creates a private field behind the scenes

```c#
private string <Name>k__BackingField;
```

## Read-only (init-only) properties

### C#6+: get-only auto-property

```c#
public class Point
{
	// Can only be set in the constructor
	public int X { get; }
	public int T { get; }

	Public Point(int x, int y)
	{
		X = x;
		Y = y;
	}
}
```

### C#9+:init-only setter

```C#
public class Employee
{
	// Can be set during object initialization, but not afterwatds
	public int Id { get; init; }
	public string Department { get; init; }
}

// Usage
var employee = new Employee { Id = 42, Department = "HR" };
// employee.Id = 100; // compile-error
```

### Different Access Levels

Give getter and setter different accessibility. For example, a public getter but a private setter:

```C#
public class BankAccount
{
	public decimal Balance { get; private set; }

	public void Deposit(decimal amount)
	{
		if (amount <  0) throw new ArgumentException();
		Balance += amount;
	}
}
```

Code outside the class can read `Balance` but only members of `BankAccount` can change it.

### Default values with initializers

Since C#6 can provide an initial value directly:

```c#
public class Settings
{
	public bool IsEnabled { get; set; } = true;
	public int TimeoutMiliseconds { get; set; } = 5000;
}
```

### Expression-bodied auto-properties

For ultra-concise read-only properties:

```c#
public class Circle
{
	public double Radius { get; }
	public double Area => Math.PI * Radius * Radius;

	public Circle(double radius) => Radius = radius;
}
```

Here `Area` is a computed, read-only property without any setter.

### When to use--and when not

Use auto-properties when you don't need any logic in getter/setter.

Switch to full properties (with explicit backing fields) when you need to validate inputs, raise notification (e.g., `INotifyPropertyChanged`) or encapsulate complex logic.