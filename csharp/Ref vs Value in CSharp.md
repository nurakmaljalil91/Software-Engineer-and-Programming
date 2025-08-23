
## Value Types

Stored **directly in the memory** (Stack)

When assign to another variable, it **create copy**

Changing one does not affect the other

Example:

```C#
int a = 10;
int b = a; // copy of a
b = 20;

Console.WriteLine(a); // 10
Console.WriteLine(b); // 20
```

`a` remains `10` because `b`  is an independent copy

## Reference Types

Stored  as a **reference (pointer)** in memory (heap)

When assign a Reference type to another variable, both variables **point to the same object**

Changing one will **affect the another**

Example:
 - `class`
 - `string`
 - `array`
 - `object`

```C#
class Person {
    public string Name;
}

Person person1 = new Person();
person1.Name = "Akmal";

Person person2 = person1; // copy of the reference
person2.Name = "Nur Akmal";

Console.WriteLine(person1.Name); // "Nur Akmal"
Console.WriteLine(person2.Name); // "Nur Akmal"
```

both `person1` and `person2` point in the **same Person object**

## Ref  and out keywords

In C#, you can **pass parameters by reference** explicitly

**`ref`**: Passes an existing variable by reference (must be initialized before passing)

**`out`**: Passes a variable by reference, but the method must assign it before returning

Example with `ref`:

```c#
void Increment(ref int number) {
    number++;
}

int x = 5;
Increment(ref x);
Console.WriteLine(x); // 6
```

Example with `out` :

```C#
void GetValues(out int a, out int b) {
    a = 10;
    b = 20;
}

int num1, num2;
GetValues(out num1, out num2);
Console.WriteLine(num1); // 10
Console.WriteLine(num2); // 20
```

## Summary Table

|Feature|Value Type|Reference Type|
|---|---|---|
|Stored in|Stack|Heap|
|Assignment|Creates a copy|Copies reference (pointer)|
|Examples|`int`, `struct`, `bool`|`class`, `string`, `array`|
|Behavior|Independent copy|Shared object reference|
