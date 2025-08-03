## Overview

Friend class is one that explicitly grant access to private and protected members of another class.
## Examples

```c++
class B; // forward declaration

class A {
	friend class B;
private:
	int secret = 43;
protected:
	void hidden();
public:
	int getSecret() const { return secret; }
}
```

class B can now access A’s `secret` and `hidden()` even though they’re non-public.
## Syntax and Mechanics

**Declaration site**: Inside `A`, write `friend class B;`

**Scope**: Friendship is one-way. B can access A’s internals, but A doesn’t gain special access to B.

**Not inherited**: If C inherits from A, C is _not_ automatically a friend of B (and vice versa).

**Granularity**: You can also grant friendship to individual functions:

```c++
class A {
  friend void helper(A&);
  // …
};
```

**No reciprocity**: `friend` is not mutual; you must declare it separately in each class that wants to grant access.
## Why Use `friend`?

**Operator Overloading**

```c++
class BigInt {
    friend BigInt operator+(const BigInt&, const BigInt&);
    // …
private:
    std::vector<int> digits;
};
```

**Serialization / I/O**

```c++
class Point {
    friend std::ostream& operator<<(std::ostream&, const Point&);
private:
    float x, y;
};
```

**Tight Coupling Between Classes**  

When two classes are so interdependent that you’d otherwise need lots of getters/setters.

## Example: Inspector Pattern

```c++
#include <iostream>

class Box {
    friend class BoxInspector;  // Grant BoxInspector full access
private:
    int width, height, depth;

public:
    Box(int w, int h, int d)
      : width(w), height(h), depth(d) {}
};

class BoxInspector {
public:
    static void printDimensions(const Box& b) {
        // Can access private members directly:
        std::cout << "W×H×D = "
                  << b.width << "×"
                  << b.height << "×"
                  << b.depth << "\n";
    }
};

int main() {
    Box b(10, 20, 30);
    BoxInspector::printDimensions(b);
    return 0;
}
```

Here, `BoxInspector` doesn’t need any public getters because it’s a friend.
## Pitfalls & Best Practices

**Encapsulation breach**: Friendship breaks encapsulation—use sparingly and only when classes are truly tightly coupled.

**Maintenance cost**: Excessive `friend` declarations scatter implementation details.

**Prefer minimal exposure**: If you only need one function to have access, prefer a **friend function** over an entire friend class.

**Document your intent**: Clearly comment why a class is granting friendship to avoid future confusion.
### Summary

`friend` lets one class or function peek into another’s private/protected internals.

It’s unidirectional and not inherited—declare friendship wherever needed.

Use for operator-overloads, I/O, or tightly coupled helper classes—but avoid overusing it to keep your design clean.