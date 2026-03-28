---
title: Adding ChaiScript to C++ Project
category: cplusplus
tags: [#cplusplus]
created: 2026-03-28
updated: 2026-03-28
status: active
---
## Overview

[ChaiScript](https://github.com/ChaiScript/ChaiScript) is one of the only embedded scripting language designed from the ground up to directly target C++ and take advantage of modern C++ development techniques, working with the developer how they would expect it to work. Being a native C++ application, it has some advantages over existing embedded scripting languages:

1. It uses a header-only approach, which makes it easy to integrate with existing projects.
2. It maintains type safety between your C++ application and the user scripts.
3. It supports a variety of C++ techniques including callbacks, overloaded functions, class methods, and stl containers.

## Prerequisites

- [[CMake]]
- ChaiScript requires a C++17 compiler to build with support for variadic templates. It has been tested with gcc 7 and clang 6 (with libcxx).
## Install

#### Header-only version

Copy the include [folder](https://github.com/gabime/spdlog/tree/v1.x/include/spdlog) to your build tree and use a C++11 compiler.

> NOTE:
> If you using CLion Mingw, set CLion Mingw path to the environment variable first before build the spdlog

> NOTE:
> Clone the develop branch instead of master or release branch. It not supported C++ 20
#### Compiled version (recommended - much faster compile times)

```shell
$ git clone https://github.com/ChaiScript/ChaiScript.git
$ cd ChaiScript 
$ cmake -B_builds -DCMAKE_INSTALL_PREFIX="C:\Users\User\Developments\FantasyAdventure\vendors\chaiscript" -G "MinGW Makefiles" -DCMAKE_CXX_STANDARD=20
$ cmake --build _builds --target install
```

see example [CMakeLists.txt](https://github.com/gabime/spdlog/blob/v1.x/example/CMakeLists.txt) on how to use.
### Project structure:

```css
FantasyAdventure
├── CMakeLists.txt
└── src
    └── main.cpp
└── vendors
	└── chaiscript
	    |── include
		|	└── chaiscript
		|		└── chaiscript.h
		|		└── ...other_files
	    └── lib
		    └── chaiscript.a
```

## Using

Add [ChaiScript](https://github.com/ChaiScript/ChaiScript) `include` and `lib` in the `CMakeLists.txt`.

```cmake
cmake_minimum_required(VERSION 3.30)  
project(FantasyAdventure)  
set(CMAKE_CXX_STANDARD 20)  
  
# Set ChaiScript include directory  
set(CHAISCRIPT_PATH vendors/chaiscript)  
include_directories(${CHAISCRIPT_PATH}/include)  
  
# Add MinGW-specific compiler flags if needed  
if(MINGW)  
    add_compile_options(-O1 -Wa,-mbig-obj)  
endif()  
  
add_executable(FantasyAdventure src/main.cpp)
```

```c++
#include "chaiscript/chaiscript.hpp"  
  
std::string greet(const std::string &name) {  
    return "Hello, " + name + "!";  
}  
  
int main() {  
    chaiscript::ChaiScript chaiScript;  
  
    // Register a free function  
    chaiScript.add(chaiscript::fun(&greet), "greet");  
  
    // Now we can call `greet` from our script code  
    chaiScript.eval(R"(  
        var message = greet("Alice");        print(message); // "Hello, Alice!"    )");  
  
    return 0;  
}
```
