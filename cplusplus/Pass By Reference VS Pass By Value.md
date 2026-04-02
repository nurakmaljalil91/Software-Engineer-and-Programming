---
title: Pass By Reference VS Pass By Value
category: cplusplus
tags:
  - cplusplus
created: 2026-03-28
updated: 2026-03-28
status: active
---

Here’s a structured guide to help you grasp the differences between **pass-by-value** and **pass-by-reference** in C++, with examples, pros/cons, and when to use each.

---

## 1. Conceptual Overview

| Aspect          | Pass by Value                                               | Pass by Reference                                          |
| --------------- | ----------------------------------------------------------- | ---------------------------------------------------------- |
| How it works    | Function receives a **copy** of data.                       | Function receives an **alias** to data.                    |
| Syntax          | `void f(Type x);`                                           | `void f(Type& x);`                                         |
| Mutation effect | Changes inside `f` **do not** affect the caller’s variable. | Changes inside `f` **do** affect the caller’s variable.    |
| Copy cost       | May be expensive for large objects.                         | No copy, just a reference; more efficient for big objects. |
| Nullability     | Always valid (it’s a copy).                                 | Cannot be null (must bind to an existing object).          |

---

## 2. Pass by Value

```cpp
void increment(int x) {
    x += 1;             // modifies the local copy only
}

int main() {
    int a = 5;
    increment(a);
    // a is still 5 here
}
```

* **Use when**:

  * You don’t want the function to alter the caller’s data.
  * The type is small and cheap to copy (e.g., built-in types, small structs).
* **Pros**:

  * Caller’s data is safe from unintended modification.
  * No surprises about side-effects.
* **Cons**:

  * Copies can be costly for large objects (e.g., big `std::vector`, custom classes).

---

## 3. Pass by Reference

```cpp
void increment(int& x) {
    x += 1;             // modifies the original variable
}

int main() {
    int a = 5;
    increment(a);
    // a is now 6
}
```

* **Use when**:

  * You **do** want to modify the caller’s variable.
  * You want to avoid copying large objects.
* **Pros**:

  * Efficient: no copy of the argument.
  * Clear syntax (no need to dereference pointers).
* **Cons**:

  * Function can introduce side-effects by modifying arguments.
  * References must bind to valid objects (no “null” reference).

---

## 4. Const-Reference for Read-Only

```cpp
void printName(const std::string& name) {
    std::cout << name << "\n";
}
```

* **Why use?**

  * Avoid copying large types **and** protect them from modification.
* **Behavior**:

  * No copy, read-only access.

---

## 5. When to Use Which

| Scenario                                           | Recommendation               |
| -------------------------------------------------- | ---------------------------- |
| Function needs to modify the caller’s variable     | Pass by reference            |
| Function only reads data, and data is large        | Pass by const-reference      |
| Function only reads data, and data is small/simple | Pass by value                |
| You want complete safety from side-effects         | Pass by value (or const-ref) |

---

## 6. Common Pitfalls

1. **Dangling references**

   ```cpp
   int* makePtr() {
       int local = 42;
       return &local;    // BAD: local goes out of scope
   }
   ```

   Always ensure references (or pointers) remain valid.

2. **Unintended side-effects**

   ```cpp
   void reset(int& x) { x = 0; }
   reset(myVar);        // myVar is now zero—intentional?
   ```

   Be mindful when a function can alter its arguments.

3. **Confusing syntax**

   * Reference declaration: `Type& name`
   * Address-of operator: `&var`
     Don’t mix up the two.

---

## 7. Quick Recap

* **Value**: safe, isolated, but can be slow for big types.
* **Reference**: fast, can modify original, but watch out for side-effects and lifetime issues.
* **Const-Reference**: best of both worlds for read-only access to large objects.

With these points and examples in mind, you’ll be able to choose the right parameter-passing strategy for your C++ functions!
