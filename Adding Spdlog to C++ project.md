## Overview

[Spdlog](https://github.com/gabime/spdlog) is a very fast, header-only/compiled, C++ logging library

## Prerequisites

- [[CMake]]
## Install

#### Header-only version

Copy the include [folder](https://github.com/gabime/spdlog/tree/v1.x/include/spdlog) to your build tree and use a C++11 compiler.

> NOTE:
> If you using CLion Mingw, set CLion Mingw path to the environment variable first before build the spdlog

#### Compiled version (recommended - much faster compile times)

```shell
$ git clone https://github.com/gabime/spdlog.git
$ cd spdlog 
$ cmake -B_builds -DCMAKE_INSTALL_PREFIX="C:\Users\User\Developments\SharkCardGame\vendors\spdlog"  -G "MinGW Makefiles" -DCMAKE_CXX_STANDARD=17
$ cmake --build _builds --target install
```

see example [CMakeLists.txt](https://github.com/gabime/spdlog/blob/v1.x/example/CMakeLists.txt) on how to use.

### Project structure:

```css
SharkCardGame
├── CMakeLists.txt
└── src
    └── main.cpp
└── vendors
    └── entt
	    └── entt.hpp
	└── spdlog
	    |── include
		|	└── spdlog
		|		└── spdlog.h
		|		└── ...other_files
	    └── lib
		    └── libspdlog.a
```

## Using

Add [Spdlog](https://github.com/gabime/spdlog) `include` and `lib` in the `CMakeLists.txt`.

```cmake
cmake_minimum_required(VERSION 3.28)  
  
project(SharkCardGame)  
  
set(CMAKE_CXX_STANDARD 17)  
  
# set entt path  
include_directories(ENTT vendors/entt)  
  
# set spdlog path  
set(spdlog_PATH vendors/spdlog)  
include_directories(${spdlog_PATH}/include)  
link_directories(${spdlog_PATH}/lib)  
  
# Set CMAKE_PREFIX_PATH to include spdlog_DIR  
set(CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH} ${spdlog_PATH})  
  
find_package(spdlog REQUIRED)
  
add_executable(
		SharkCardGame 
		src/main.cpp  
        src/components/Player.cpp        
        src/components/Player.h
    )  
  
target_link_libraries(SharkCardGame PRIVATE spdlog::spdlog $<$<BOOL:${MINGW}>:ws2_32>)
```

```c++
#include <iostream>  
#include "entt/entt.hpp"  
#include "spdlog/spdlog.h"  
#include "components/player.h"  
  
int main() {  
    spdlog::info("Welcome to spdlog!");  
  
    entt::registry registry;  
  
    auto entity = registry.create();  
  
    registry.emplace<Player>(entity, "Nur Akmal Jalil");  
  
    auto view = registry.view<Player>();  
  
    for (auto entity : view) {  
        auto &player = view.get<Player>(entity);  
        spdlog::info("Player: {} with score {}", player.getName(), player.getScore());  
    }  
  
    return 0;  
}
```
## References

- Use this guide to install [Spdlog](https://github.com/gabime/spdlog): [https://github.com/gabime/spdlog/wiki/9.-CMake](https://github.com/gabime/spdlog/wiki/9.-CMake)