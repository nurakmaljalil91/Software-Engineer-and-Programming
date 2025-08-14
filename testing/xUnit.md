## Overview

- **Scope:** a single unit (usually one public method).
- **Goal:** verify behavior (not implementation details).
- **Pattern:** **Arrange–Act–Assert (AAA)**.
- **Fast & isolated:** no real DB, file system, network.

## Create a test project

```bash
# from your solution folder
dotnet new xunit -n MyApp.Tests
dotnet sln add MyApp.Tests/MyApp.Tests.csproj

# reference the project you want to test
dotnet add MyApp.Tests/MyApp.Tests.csproj reference src/MyApp/MyApp.csproj

# run tests
dotnet test
```

## First test: structure and naming

```csharp
// File: CalculatorTests.cs
using Xunit;

public class Calculator
{
    public int Add(int a, int b) => a + b;
}

public class CalculatorTests
{
    [Fact] // a single example-based test
    public void Add_ReturnsSum_WhenGivenTwoIntegers()
    {
        // Arrange
        var calc = new Calculator();

        // Act
        var result = calc.Add(2, 3);

        // Assert
        Assert.Equal(5, result);
    }
}
```

**Naming tip:** `MethodName_StateUnderTest_ExpectedBehavior`.

## Theories (data-driven tests)

Use `[Theory]` with `[InlineData]` / `[MemberData]` to cover multiple cases succinctly.

```csharp
public class CalculatorTheoryTests
{
    [Theory]
    [InlineData(1, 2, 3)]
    [InlineData(0, 0, 0)]
    [InlineData(-2, 5, 3)]
    public void Add_WorksForSeveralPairs(int a, int b, int expected)
    {
        var calc = new Calculator();
        var result = calc.Add(a, b);
        Assert.Equal(expected, result);
    }
}
```

## Testing async methods

```csharp
public interface IClock { DateTime UtcNow { get; } }

public class GreetingService
{
    private readonly IClock _clock;
    public GreetingService(IClock clock) => _clock = clock;

    public Task<string> GreetAsync(string name)
    {
        var hour = _clock.UtcNow.Hour;
        var prefix = hour < 12 ? "Good morning" : "Hello";
        return Task.FromResult($"{prefix}, {name}!");
    }
}

public class GreetingServiceTests
{
    [Fact]
    public async Task GreetAsync_UsesTimeFromClock()
    {
        var fakeClock = new FakeClock(new DateTime(2025, 1, 1, 9, 0, 0, DateTimeKind.Utc));
        var svc = new GreetingService(fakeClock);

        var msg = await svc.GreetAsync("Akmal");

        Assert.Equal("Good morning, Akmal!", msg);
    }

    private sealed class FakeClock : IClock
    {
        public FakeClock(DateTime fixedNow) => UtcNow = fixedNow;
        public DateTime UtcNow { get; }
    }
}
```

**Takeaway:** inject dependencies so you can substitute **test doubles** (fakes/mocks).
## Using Moq for mocks (popular choice)

```csharp
// dotnet add MyApp.Tests package Moq
using Moq;
using Xunit;

public interface IEmailSender { Task SendAsync(string to, string body); }

public class Notifier
{
    private readonly IEmailSender _email;
    public Notifier(IEmailSender email) => _email = email;

    public async Task NotifyAsync(string userEmail, string message)
    {
        if (string.IsNullOrWhiteSpace(userEmail)) throw new ArgumentException("userEmail");
        await _email.SendAsync(userEmail, message);
    }
}

public class NotifierTests
{
    [Fact]
    public async Task NotifyAsync_SendsEmail()
    {
        // Arrange
        var emailMock = new Mock<IEmailSender>();
        var sut = new Notifier(emailMock.Object);

        // Act
        await sut.NotifyAsync("a@b.com", "hello");

        // Assert – verify interaction
        emailMock.Verify(e => e.SendAsync("a@b.com", "hello"), Times.Once);
    }

    [Fact]
    public async Task NotifyAsync_Throws_WhenEmailMissing()
    {
        var emailMock = new Mock<IEmailSender>();
        var sut = new Notifier(emailMock.Object);

        var ex = await Assert.ThrowsAsync<ArgumentException>(() => sut.NotifyAsync("", "x"));
        Assert.Equal("userEmail", ex.ParamName);
    }
}
```

**Rules of thumb with mocks:**
- Mock **behavioral** collaborators (e.g., an SMTP client).
- Avoid over-verifying (test the outcome more than the choreography).
## Exceptions, collections, and floating points

```csharp
// Exceptions
Assert.Throws<InvalidOperationException>(() => DoBadThing());

// Collections
Assert.Equal(new[] {1,2,3}, list);            // same sequence
Assert.Contains(2, list);                     // membership

// Floating point (tolerance)
Assert.Equal(3.14, value, precision: 2);      // or Assert.InRange(...)
```
## Shared context with Fixtures

Use **fixtures** to create expensive objects once per test class or per collection.

```csharp

```