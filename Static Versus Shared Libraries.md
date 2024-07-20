## Overview

Static and shared libraries are two common types of libraries used in [[C++]] (and other programming languages). Each type has its own advantages and disadvantages. Here's a detailed comparison:

### Static Libraries

#### Definition

- A static library (typically `.a` on Unix-like systems or `.lib` on Windows) is a collection of object files that are linked into the program at compile time.

#### Characteristics

- **Compilation**: The library code is included directly into the executable at compile time.
- **Distribution**: Only the final executable is needed to run the program; the library itself is not required at runtime.
- **Size**: The executable size is larger because it contains all the code from the static libraries it uses.
- **Performance**: May have slightly better performance since all code is already in the executable and does not require dynamic linking at runtime.
- **Versioning**: Easier to manage versioning as the executable is self-contained.
- **Duplication**: If multiple programs use the same static library, each program will have its own copy of the library code, leading to possible code duplication.

#### Use Case

- Useful for small programs or utilities where distribution simplicity is preferred over storage efficiency.
- Commonly used in embedded systems where runtime linking is not practical.

### Shared (Dynamic) Libraries

#### Definition

- A shared library (typically `.so` on Unix-like systems or `.dll` on Windows) is a collection of object files that are linked at runtime.

#### Characteristics

- **Compilation**: The library code is not included in the executable at compile time. Instead, the executable contains references to the library.
- **Distribution**: Both the executable and the shared library are needed to run the program.
- **Size**: The executable size is smaller because it does not contain the code from the shared libraries.
- **Performance**: May have slightly slower performance due to the overhead of dynamic linking at runtime, although this is often negligible.
- **Versioning**: More complex as the shared library must be compatible with the executable. Care must be taken to avoid "DLL Hell" where incompatible versions of a shared library are present.
- **Memory Usage**: More efficient if multiple programs use the same shared library, as the library code is loaded into memory only once and shared among all programs.

#### Use Case

- Useful for large applications where multiple programs use the same libraries, reducing overall memory usage and storage space.
- Preferred for systems where modularity and ease of updates are important. Shared libraries can be updated independently of the executable.

### Example

#### Static Library Example

**Creating Static Library:**

```bash
g++ -c foo.cpp -o foo.o 
ar rcs libfoo.a foo.o
```

**Linking with Static Library:**

```bash
g++ main.cpp -L. -lfoo -o main
```

#### Shared Library Example

**Creating Shared Library:**

```bash
g++ -c -fPIC foo.cpp -o foo.o 
g++ -shared -o libfoo.so foo.o
```

**Linking with Shared Library:**

```bash
g++ main.cpp -L. -lfoo -o main 
export LD_LIBRARY_PATH=.:$LD_LIBRARY_PATH  # Unix-like systems ./main
```
### Summary

**Static Libraries:**

- **Pros**: Simplicity in distribution, easier version management, no dependency issues at runtime.
- **Cons**: Larger executable size, code duplication, harder to update library code independently.

**Shared Libraries:**

- **Pros**: Smaller executable size, better memory efficiency, easier to update library code independently.
- **Cons**: Requires handling of library dependencies at runtime, potential versioning issues.

The choice between static and shared libraries depends on the specific requirements of your project, including considerations for distribution, memory usage, and update management.