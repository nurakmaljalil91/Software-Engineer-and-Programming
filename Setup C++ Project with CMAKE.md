- Install [[CMake]] library from https://cmake.org/. Version must be 3.0+ and can be checked by `--version`

```bash
> cmake --version
cmake version 3.3.1

CMake suite maintained and supported by Kitware (kitware.com/cmake).
```

- Create a file in the project `src` directory named `main.cpp`

```c++
#include <iostream>  
  
int main() {  
    std::cout << "Hello, World!" << std::endl;  
    return 0;  
}
```

- In root folder create a `CMakeLists.txt` file. For a simple project, only three commands is all that is required.
- Lower case is preferred when writing the commands
- Any project must be included with `cmake_minimum_required()`, `project()` and `add_executable()`

```CMakeLists.txt
cmake_minimum_required(VERSION 3.28)  
project(BasicProject)  
  
set(CMAKE_CXX_STANDARD 17)  
  
add_executable(BasicProject src/main.cpp)
```

- Directory structure

```css
SimpleProject
├── CMakeLists.txt
└── src
    └── main.cpp
```
- Generate the Build System with [[CMake]] .From the root of the `MyProject` directory, run the following commands:

```bash
mkdir build 
cd build 
cmake ..
```

- This will generate the necessary build files in the `build` directory.    
- After generating the build files, run the following command to build the project:

```bash
cmake --build .
```
- Once the project is built, you can run the executable:

```bash
cd Debug
./SimpleProject.exe
```

- You should see the output:

```bash
Hello, World!
```

- Read more on this in the [[CMake]] official tutorial [CMAKE Exercise 1 building a basic project](https://cmake.org/cmake/help/latest/guide/tutorial/A%20Basic%20Starting%20Point.html#exercise-1-building-a-basic-project)


## Building for Linux

- From the root of the `SimpleProject` directory, run the following commands:

```bash
mkdir build 
cd build 
cmake .. -G "Unix Makefiles"
```

- This will generate the necessary Makefiles in the `build` directory. The `-G "Unix Makefiles"` option specifies that [[CMake]] should generate Makefiles.
- After generating the Makefiles, run the following command to build the project:

```bash
make
```

- Once the project is built, you can run the executable:

```bash
./SimpleProject.exe
```

## Alternative: Using Ninja

- If you prefer using [Ninja](https://github.com/ninja-build/ninja), you can replace the `-G "Unix Makefiles"` with `-G Ninja` in the [[CMake]] command:

```bash
mkdir build 
cd build 
cmake .. -G Ninja 
ninja
```

Then run the executable with:

```bash
./SimpleProject.exe
```