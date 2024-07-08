## Overview

`EnTT` is a header-only, tiny and easy to use library for game programming and much more written in **modern C++**.  [Among others](https://github.com/skypjack/entt/wiki/EnTT-in-Action), it's used in [**Minecraft**](https://minecraft.net/en-us/attribution/) by Mojang, the [**ArcGIS Runtime SDKs**](https://developers.arcgis.com/arcgis-runtime/)by Esri and the amazing [**Ragdoll**](https://ragdolldynamics.com/).  

## Using single include

Clone the repository from [https://github.com/skypjack/entt](https://github.com/skypjack/entt)

```bash
git clone https://github.com/skypjack/entt
```

Copy the `entt.hpp` from `single_include` folder to your `vendors/entt/entt` project.

```css
SimpleProject
├── CMakeLists.txt
└── src
    └── main.cpp
└── vendors
    └── entt
	    └── entt.hpp
```

Add the `enTT` library to the `CMakeLists.txt`.

```cmake
cmake_minimum_required(VERSION 3.28)  
  
project(SharkCardGame)  
  
set(CMAKE_CXX_STANDARD 17)  
  
# set entt path  
include_directories(ENTT vendors/entt)  
  
add_executable(SharkCardGame 
		src/main.cpp  
        src/components/Player.cpp        
        src/components/Player.h
    )
```

Simple example of usage.

```c++
#include <iostream>  
#include "entt/entt.hpp"  
#include "components/player.h"  
  
int main() {    
    entt::registry registry;  
  
    auto entity = registry.create();  
  
    registry.emplace<Player>(entity, "Nur Akmal Jalil");  
  
    auto view = registry.view<Player>();  
  
    for (auto entity : view) {  
        auto &player = view.get<Player>(entity);  
        std::cout << "Entity with name " << player.getName() << std::endl;  
    }  
  
    return 0;  
}
```




