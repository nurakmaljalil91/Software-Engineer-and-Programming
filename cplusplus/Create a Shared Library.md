## Overview

Here's a step-by-step guide to creating a shared library in [[C++]] using MinGW-w64 and [[CMake]], including how to set up the project structure, build the library, and use it in an application. This guide will also cover installing the necessary files for reuse in other projects.

### Step-by-Step Guide to Creating and Using a Shared Library with MinGW-w64 and [[CMake]]

#### Project Structure

Start by organizing your project directory structure as follows:

```css
MyConsole
├── CMakeLists.txt
└── src
    └── Console.h
    └── Console.cpp
```

Create `Console.h`

```cpp
#ifndef CONSOLE_H
#define CONSOLE_H

#include <string>

class Console {
public:
    static void WriteLine(const std::string& message);
};

#endif // CONSOLE_H
```

Create `Console.cpp`

```cpp
#include "console.h" 
#include <iostream> 

void Console::WriteLine(const std::string& message) { 
	std::cout << message << std::endl; 
}
```

Create `CMakeLists.txt` file

```cmake
cmake_minimum_required(VERSION 3.10)

# Set the project name and version
project(Console VERSION 1.0)

# Specify the library to be built
add_library(Console SHARED src/Console.cpp)

# Specify the include directories
target_include_directories(Console PUBLIC ${PROJECT_SOURCE_DIR})

# Set the C++ standard
set_target_properties(Console PROPERTIES CXX_STANDARD 11 CXX_STANDARD_REQUIRED YES)

# Specify installation rules
install(TARGETS Console
        RUNTIME DESTINATION bin           # for DLLs
        LIBRARY DESTINATION lib           # for shared libraries
        ARCHIVE DESTINATION lib/static)   # for static libraries and import libraries

install(FILES src/Console.h DESTINATION include/Console)
```

### Build and Install the Library

Navigate to your project root directory and create a build directory:
```bash
cd /path/to/project_root 
mkdir build 
cd build
```

Run [[CMake]] to configure the project:

```bash
cmake -G "MinGW Makefiles" DCMAKE_INSTALL_PREFIX="./install" ..
```

Build and install the library:    

```bash
cmake --build . --target install
```
#### Verify the Installation

After running the build and install commands, check the `build/install` directory. It should contain the following structure:

```css
install
└── include
|    └── Console
|	    └── Console.h
└── lib
|	└── Console.dll.a
|	└── cmake
|		└── Console
|			└── ConsoleConfig.cmake
|			└── ConsoleTargets.cmake
└── bin
	└── Console.dll
```

### Use the Installed Library in Other Projects

In your other projects, you can now use the installed library by finding the package with CMake.

**Example `other_project/CMakeLists.txt`**


```cmake
cmake_minimum_required(VERSION 3.10)  
# Set the project name 
project(OtherApp)  

# Add the executable 
add_executable(OtherApp main.cpp)  

# Find the installed library package 

find_package(Console REQUIRED PATHS /path/to/project_root/build/install)  

# Link the shared library 
target_link_libraries(OtherApp Console::Console)  

# Include the directory containing the header file 
target_include_directories(OtherApp PUBLIC ${Console_INCLUDE_DIRS})
```

By following this guide, you will have successfully created a shared library, installed it, and made it reusable for other projects.