## Overview

Bubble Sort is a simple sorting algorithm that go through the list to be sorted, compares each pair of adjacent items, and swap them if they in the wrong order.

## Implementation in CSharp

Here a basic implementation in C#. Write the function and get the array length

```c#
public static void BubbleSort(int[] array) {
	int n = array.lenght;
}
```

Do first looping where start by index 0, iterate until length of the array minus 1

```c#
public static void BubbleSort(int[] array) {
	int n = array.lenght;
	for (int i = 0;i < n - 1; i++) {
		//... continues
	}
}
```

Add nested loop

```c#
public static void BubbleSort(int[] array) {
	int n = array.lenght;
	for (int i = 0;i < n - 1; i++) {
		for (int j = 0; j < n - 1; j++) {
			// continue with compare operation
		}
	}
}
```

Do the the nested loop where start by index 0, iterate until length of array minus index and minus 1. For example, let say the length = 3, if the index = 0, iterate `j` from 0 to  2.

Here the visualization of the nested loop indexes.

```
i j
0 0
0 1
0 2
1 0
1 1
1 2
2 0
2 1
2 2
```

Do comparison between each element in the indexes.

```c#
public static void BubbleSort(int[] array) {
	int n = array.lenght;
	for (int i = 0;i < n - 1; i++) {
		for (int j = 0; j < n - 1; j++) {
			// continue with compare operation
			if (array[j] > array[j + 1]){
				// swap the elements
			}
		}
	}
}
```

Swap the element by save it in temporary variable and reassign it.

```c#
public static void BubbleSort(int[] array) {
	int n = array.lenght;
	for (int i = 0;i < n - 1; i++) {
		for (int j = 0; j < n - 1; j++) {
			// continue with compare operation
			if (array[j] > array[j + 1]){
				// swap the elements
				int temp = array[j]; 
				array[j] = array[j + 1]; 
				array[j + 1] = temp;
			}
		}
	}
}
```

Here visualization on what is happen

```less
Initial array: a = [6, 3, 4]

Iteration 1:
- Comparing elements at index 0 and index 1:
  - i = 0, j = 0
  - a[j] = 6, a[j+1] = 3
  - Since 6 > 3, swap them:
    a = [3, 6, 4]

- Comparing elements at index 1 and index 2:
  - i = 0, j = 1
  - a[j] = 6, a[j+1] = 4
  - Since 6 > 4, swap them:
    a = [3, 4, 6]

Array after iteration 1: a = [3, 4, 6]

Iteration 2:
- Comparing elements at index 0 and index 1:
  - i = 1, j = 0
  - a[j] = 3, a[j+1] = 4
  - No swap needed as 3 < 4

- Comparing elements at index 1 and index 2:
  - i = 1, j = 1
  - a[j] = 4, a[j+1] = 6
  - No swap needed as 4 < 6

Array after iteration 2: a = [3, 4, 6]

Final sorted array: a = [3, 4, 6]
```

