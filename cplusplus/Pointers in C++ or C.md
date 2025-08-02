# Overview

Variable that store the memory address of another variable.

## Example Scenario

Declare three variables.

```c
int var1 = 10; 
int var2 = 20; 
int *ptr = &var1;
```

Memory layout

```scss
Memory Address      |  Value
--------------------|--------
0x1000              |  10  (var1)
0x1004              |  20  (var2)
0x1008              |  0x1000  (ptr)
```

Diagram

```lua
   +--------+       +--------+       +--------+
   |  var1  |       |  var2  |       |  ptr   |
   |        |       |        |       |        |
   |   10   |       |   20   |       | 0x1000 |
   +--------+       +--------+       +--------+
   (0x1000)         (0x1004)         (0x1008)
       ^                
       |                
       |                
       +-------------------+
                           |
                       +--------+
                       |  var1  |
                       |        |
                       |   10   |
                       +--------+
                       (0x1000)
```

## Explanation

1. **Memory Addresses**:
    - `var1` is stored at memory address `0x1000` and holds the value `10`.
    - `var2` is stored at memory address `0x1004` and holds the value `20`.
    - `ptr` is stored at memory address `0x1008` and holds the address of `var1` (`0x1000`).
2. **Pointer**:
    - The pointer `ptr` contains the address of `var1`, which means `ptr` is pointing to the memory location where `var1` is stored.
    - Dereferencing `ptr` (`*ptr`) will give the value stored at `var1`'s address, which is `10`.