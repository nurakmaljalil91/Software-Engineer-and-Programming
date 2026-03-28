---
title: Adding Assimp to C++ Project
category: cplusplus
tags: [#cplusplus]
created: 2026-03-28
updated: 2026-03-28
status: active
---
## Overview

Model loader

## Getting Started

```bash
git clone https://github.com/assimp/assimp.git
```

```bash
cmake -S . -B _builds -G Ninja -DCMAKE_MAKE_PROGRAM="C:\Users\User\AppData\Local\Programs\CLion\bin\ninja\win\x64\ninja.exe" -DCMAKE_BUILD_TYPE=Debug -DCMAKE_C_COMPILER="C:\Users\User\AppData\Local\Programs\CLion\bin\mingw\bin\gcc.exe" -DCMAKE_CXX_COMPILER="C:\Users\User\AppData\Local\Programs\CLion\bin\mingw\bin\g++.exe" -DASSIMP_BUILD_ASSIMP_TOOLS=OFF -DASSIMP_BUILD_TESTS=OFF -DASSIMP_INSTALL=ON -DCMAKE_INSTALL_PREFIX="C:\Users\User\Developments\FantasyTactics\vendors\assimp"
```

```bash
cmake --build _builds --target install
```