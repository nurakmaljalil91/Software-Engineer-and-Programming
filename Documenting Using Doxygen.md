## Overview

Generate documentation for [[C++]] using [Doxygen](https://www.doxygen.nl/index.html)

## Installing Doxygen

Go to [https://www.doxygen.nl/download.html](https://www.doxygen.nl/download.html)

Download and install for [[Windows]] 11

![[deoxygen-download.png]]

Do full install (include Graphviz)

**On macOS**:

```bash
brew install doxygen graphviz
```

**On Linux:**

```bash
sudo apt-get install doxygen graphviz
```

## Create a Doxyfile

In your project root, run:

```bash
doxygen -g
```

This produces a `Doxyfile` with hundreds of options. At minimum, edit:

```ini
PROJECT_NAME           = "MyProject"
OUTPUT_DIRECTORY       = docs
INPUT                  = src include
FILE_PATTERNS          = *.cpp *.h
RECURSIVE              = YES

# Extract private members?
EXTRACT_PRIVATE        = YES
EXTRACT_STATIC         = YES

# Enable graphs
HAVE_DOT               = YES
DOT_IMAGE_FORMAT       = svg

# Generate HTML and/or LaTeX
GENERATE_HTML          = YES
GENERATE_LATEX         = NO
```

## Write Deoxygen-style Comments

[[Deoxygen]] recognize two main block types:

- JavaDoc-style `(/**...*/)`
- Qt-style `(/*!...*/)`
### File header

At the top of each header or source file:

```cpp
/**
 * @file   Vector3.h
 * @breif  3D vector with basic arhimetric
 * @author Nur Akmal Jalil
 * @date   2025-07-16
/*
```

### Class documentation

Just above the class:

```cpp
/**
 * @class Vector3
 * @brief Simple 3D vector type.
 *
 * Supports addition, subtraction, dot/cross product,
 * normalization, and scaling.
 */
class Vector3 {
public:
    // …
};
```

### Member functions

Place immediately before the declaration:

```cpp
/**
 * @brief   Compute the dot product.
 * @param   other  Right-hand side vector.
 * @return  Scalar dot product.
 */
float dot(const Vector3& other) const;
```

### Parameters, return, and templates

```cpp
/**
 * @brief   Linearly interpolate between two vectors.
 * @tparam  T    Numeric type (float, double, …).
 * @param   a    Start vector.
 * @param   b    End vector.
 * @param   t    Fraction between 0 (→a) and 1 (→b).
 * @return        Interpolated vector: a*(1–t) + b*t.
 */
template<typename T>
Vector3 interpolate(const Vector3& a, const Vector3& b, T t);
```

## Organize with Groups and Modules

You can group related classes/functions into modules:

```cpp
/**
 * @defgroup MathLib  Math Library
 * @brief       Core math types and algorithms.
 * @{
 */

/** … your Vector3, Matrix4x4, etc. documentation … */

/** @} */  // end of MathLib
```

In your `Doxyfile`, enable:

```ini
ENABLE_PREPROCESSING = YES
MACRO_EXPANSION      = YES
```

## Run Doxygen & Inspect Output

```bash
doxygen Doxyfile
```

**HTML** ends up under `docs/html/index.html`

Browse that file in your browser to see the generated API reference, diagrams, and search index




