---
title: Using Conan C++ Package Manager
category: cplusplus
tags:
  - cplusplus
created: 2026-03-28
updated: 2026-03-28
status: active
---
## Install Conan

```bash
py -m pip install --upgrade conan
conan --version
```

## Overview

  

- https://conan.io/center/

-  Conan, the C/C++ Package Manager

- [[CMake]] or [[Premake]] need to generate Visual Studio Solution
## Installations

- Use `pip`  to install [[Conan]]

```bash
pip install conan
```

- Run [[Conan]] to determine it install correctly
## Starting new project

- Create `conanfile.txt` int the project folder
- Write `[requires]` section to specifies external library to install and build it
- To generate use `[generator]` section
- Example

```text
[requires]
spdlog/1.11.0

[tool_requires]
cmake/3.22.6

[generators]
CMakeDeps
CMakeToolchain
```

- Run this command  to install all library:

```bash
conan install . --build=missing
```

- You can use  to remove:

```bash
conan remove <library-name>
```

- Generate those files in the folder _build_. To do that, run:

```bash
conan install . --output-folder=build --build=missing

conan install . --output-folder=build-ninja --build=missing
```

- Now ready to build and run our simple app:

```bash
cd build

cmake .. -G "Ninja" --toolchain conan_toolchain.cmake  -DCMAKE_EXPORT_COMPILE_COMMANDS=1

cmake .. -G "Visual Studio 17 2022" --toolchain conan_toolchain.cmake

cmake --build . --config Release
```