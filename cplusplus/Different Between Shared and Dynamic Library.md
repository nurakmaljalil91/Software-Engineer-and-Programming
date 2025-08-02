In [[C++]], **static libraries** and **dynamic libraries** (sometimes called “shared libraries”) are two different ways of packaging compiled code that other programs can use.
## Static Libraries

1. **File format**: On Windows, static libraries typically end with `.lib`; on Linux/macOS, they typically end with `.a`.
2. **Linked at build time**: When you link your program with a static library, the linker copies the necessary object code **directly into** your final executable.
3. **No separate file at runtime**: After linking, your program does not need the `.a` or `.lib` file, because its contents are already baked into your `.exe` or binary.
4. **Pros**:
    - Simpler deployment: your executable is self-contained (you don’t need to ship the library separately).
    - Possibly faster startup: because no external dynamic loading is needed at runtime.
5. **Cons**:
    - Larger executables: The compiled code is duplicated in every program that links against the library.
    - Less flexible updates: If you need to fix or update the library, you must recompile and redistribute the entire application.
## Dynamic (Shared) Libraries

1. **File format**: On Windows, shared libraries are `.dll` (plus an import `.lib` to help link); on Linux they are `.so` (shared object), and on macOS they are `.dylib`.
2. **Linked at runtime**: Your application references the functions/classes in the shared library, but the actual code is **loaded** from the `.dll/.so/.dylib` **when the program runs**.
3. **Separate file at runtime**: You must ship both the main executable and the shared library files. They need to be located where the OS can find them at runtime.
4. **Pros**:
    - Smaller executables: multiple programs can share the same library in memory.
    - Easier updates: you can upgrade the library without recompiling the entire application, so long as the API/ABI remains compatible.        
5. **Cons**:
    - More complex deployment: you must ensure the library is present on the user’s system (and in the correct version) or your program may fail to load.
    - Slightly slower startup or function calls: calling code in a DLL/so can be marginally slower due to runtime indirections.
## Summary of Key Differences

| Aspect             | Static Library                           | Dynamic (Shared) Library                                          |
| ------------------ | ---------------------------------------- | ----------------------------------------------------------------- |
| File Extension     | `.lib` (Windows), `.a` (Linux/macOS)     | `.dll` + import `.lib` (Windows), `.so` (Linux), `.dylib` (macOS) |
| Linking Time       | **Build time** (code is copied into EXE) | **Runtime** (code remains in separate file)                       |
| Deployment         | Just the executable                      | Executable **plus** the `.dll/.so/.dylib` file                    |
| Update Flexibility | Re-link/rebuild required for updates     | Can update library file without rebuilding the app                |
| Binary Size        | Typically larger                         | Typically smaller                                                 |
| Memory Usage       | Each application has its own copy        | Shared among multiple processes                                   |

Which you choose depends on whether you want a self-contained executable (static) or the ability to update/share the library across multiple applications (dynamic).