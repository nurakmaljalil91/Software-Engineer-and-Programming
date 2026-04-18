---
title: Explicit and Implicit conversion
category: cplusplus
tags:
  - cplusplus
created: 2026-03-28
updated: 2026-03-28
status: active
---
When a constructor or conversion operator is not explicit, the compiler is allowed to convert values automatically. When it is explicit, you must write the conversion yourself.

Examples:

```cpp
struct Vector {
	Vector(const glm::vec3&);   // not explicit
	operator glm::vec3&();      // not explicit
};

glm::vec3 g{1,2,3};
Vector v = g;       // OK if ctor is NOT explicit
glm::vec3 g2 = v;   // OK if conversion operator is NOT explicit
```

If you mark them explicit:

```cpp
explicit Vector(const glm::vec3&);
explicit operator glm::vec3&();
```

then you must write:
  
```cpp
Vector v(g);                    // OK
glm::vec3 g2 = static_cast<glm::vec3&>(v); // OK
```
## Why CLion suggests adding explicit

CLion is warning about implicit conversions because they can cause surprising behavior. For most types, that’s good advice. But for your thin wrapper design, you want implicit conversions so glm functions accept your type without extra boilerplate. So you should ignore that suggestion in Vector/Color.

If you want to reduce CLion warnings, you can leave the glm‑to‑Color constructor explicit and keep the conversion operator implicit, but then you lose implicit construction from `glm::vec4`.

In short:

- Thin wrapper → don’t use explicit on the glm constructor or conversion operators.
- Strict wrapper → use explicit to force manual conversions.