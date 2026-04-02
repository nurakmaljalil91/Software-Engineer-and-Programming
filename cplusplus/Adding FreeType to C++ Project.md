---
title: Adding FreeType to C++ Project
category: cplusplus
tags:
  - cplusplus
created: 2026-03-28
updated: 2026-03-28
status: active
---

Go to https://freetype.org/download.html

Click on Download and click on https://sourceforge.net/projects/freetype/files/

Download latest version

Unzip the file

Open terminal at the root of unzip file and run this command to build it

```bash
cmake -B_builds -DCMAKE_INSTALL_PREFIX="C:\Users\User\Developments\FantasyTactics\vendors\freetype"  -G "MinGW Makefiles" -DCMAKE_CXX_STANDARD=17 -DFT_DISABLE_HARFBUZZ=ON
```

Install the library to the target directory

```bash
cmake --build _builds --target install
```