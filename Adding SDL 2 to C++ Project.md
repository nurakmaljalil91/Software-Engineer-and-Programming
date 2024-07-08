## Overview

[Simple DirectMedia Layer](https://www.libsdl.org/) is a cross-platform development library designed to provide low level access to audio, keyboard, mouse, joystick, and graphics hardware via OpenGL and Direct3D.

## Download

Download releases from [https://github.com/libsdl-org/SDL/releases](https://github.com/libsdl-org/SDL/releases). Choose the latest release and download the one with `SDL2-devel-2.*.*-mingw.zip`.

## Setup

Unzip the downloaded `zip` file. Go to folder `x86_64-w64-mingw32` and copy `lib`, `bin`,`share` and `include` folder. Create a `SDL2` inside the `vendors` folder in the project.

> Warning
> Somehow only `x86_64-w64-mingw32` folder is working

```css
SharkCardGame
├── CMakeLists.txt
└── src
|   └── main.cpp
└── vendors
    └── ..other_libraries_folders
	└── SDL2
	    |── include
		|	└── SDL2
		|		└── SDL.h
		|		└── ...other_files
	    └── lib
		|   └── libSDL2.a
		|── bin
		└── share
```

Add library path to the `CMakeLists.txt`.

```cmake
# set sdl2 path  
set(SDL2_PATH vendors/SDL2)  
```

Add `include` libraries.

```cmake
include_directories(${SDL2_PATH}/include)  
```

Add `lib` library.

```cmake
link_directories(${SDL2_PATH}/lib)  
```

Copy `SDL2.dll` to the `exe` folder destination.

```cmake
file(COPY ${SDL2_PATH}/bin/SDL2.dll DESTINATION ${CMAKE_BINARY_DIR})
```

Link the SDL2 `lib` to the project. Add `mingw32`,`SDL2.main` and `SDL2`.

```cmake
target_link_libraries(SharkCardGame PRIVATE mingw32 SDL2main SDL2 spdlog::spdlog $<$<BOOL:${MINGW}>:ws2_32>)
```

Here the complete file:

```cmake
cmake_minimum_required(VERSION 3.28)  
  
project(SharkCardGame)  
  
set(CMAKE_CXX_STANDARD 17)  
  
# set entt path  
include_directories(ENTT vendors/entt)  
  
# set spdlog path  
if (NOT TARGET spdlog)  
    # Stand-alone build  
    set(spdlog_DIR vendors/spdlog/include)  
    find_package(spdlog REQUIRED)  
endif ()  
  
# set sdl2 path  
set(SDL2_PATH vendors/SDL2)  
include_directories(${SDL2_PATH}/include)  
link_directories(${SDL2_PATH}/lib)  
file(COPY ${SDL2_PATH}/bin/SDL2.dll DESTINATION ${CMAKE_BINARY_DIR})  
  
add_executable(SharkCardGame src/main.cpp  
        src/components/Player.cpp        src/components/Player.h        src/components/Card.cpp        src/components/Card.h        src/components/Deck.cpp        src/components/Deck.h)  
  
target_link_libraries(SharkCardGame PRIVATE mingw32 SDL2main SDL2 spdlog::spdlog $<$<BOOL:${MINGW}>:ws2_32>)
```

> Notes
> if it not working remove the `debug` folder, reload the `CMakefileList.txt` and run again


## Simple code to test if it working

```c++
#include <iostream>  
#include "SDL2/SDL.h"  

int main(int argc, char* argv[]) {  
    if (SDL_Init(SDL_INIT_VIDEO) != 0) {  
        std::cerr << "SDL_Init Error: " << SDL_GetError() << std::endl;  
        return 1;  
    }  
  
    SDL_Window *win = SDL_CreateWindow("Hello SDL", 100, 100, 640, 480, SDL_WINDOW_SHOWN);  
    if (win == nullptr) {  
        std::cerr << "SDL_CreateWindow Error: " << SDL_GetError() << std::endl;  
        SDL_Quit();  
        return 1;  
    }  
  
    SDL_Renderer *ren = SDL_CreateRenderer(win, -1, SDL_RENDERER_ACCELERATED | SDL_RENDERER_PRESENTVSYNC);  
    if (ren == nullptr) {  
        std::cerr << "SDL_CreateRenderer Error: " << SDL_GetError() << std::endl;  
        SDL_DestroyWindow(win);  
        SDL_Quit();  
        return 1;  
    }  
  
    SDL_SetRenderDrawColor(ren, 255, 0, 0, 255);  
    SDL_RenderClear(ren);  
    SDL_RenderPresent(ren);  
  
    SDL_Delay(2000);  
  
    SDL_DestroyRenderer(ren);  
    SDL_DestroyWindow(win);  
    SDL_Quit();  
  
    return 0;  
}
```