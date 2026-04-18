---
title: Algorithms
category: algorithms
tags:
  - algorithms
created: 2026-03-28
updated: 2026-03-28
status: active
---

Coding interviews often test your ability to reason about efficiency and choose appropriate data structures.  Below is a concise review of fundamental topics.
## Big‑O notation

Big‑O describes how an algorithm’s runtime or space requirement grows with input size *n*.  Common complexities include:

- **O(1)** – constant time (e.g., array indexing).

- **O(log n)** – logarithmic time (e.g., binary search).  The input is repeatedly divided by two.

- **O(n)** – linear time (e.g., scanning an unsorted array).

- **O(n log n)** – typical of efficient comparison sort algorithms (e.g., mergesort, heapsort).

- **O(n²)** – quadratic time (e.g., naive nested‑loop solutions like bubble sort).

Understanding complexity helps you choose the right approach and argue about performance.
## Data structures

- **Arrays & lists** – contiguous memory, O(1) access by index; inserting or deleting in the middle is O(n).

- **Linked lists** – nodes that hold data and pointers; O(1) insertion/deletion at the ends, but no random access.

- **Stacks** – LIFO structure with push/pop; can be implemented using an array or linked list.  Used for DFS, backtracking.

- **Queues** – FIFO structure with enqueue/dequeue; used for BFS and scheduling.

- **Hash tables** – store key/value pairs; average O(1) insertion, lookup and deletion; collisions handled via chaining or open addressing.

- **Trees** – hierarchical structures.  Binary search trees allow O(log n) search, insertion and deletion if balanced; heaps support efficient priority queue operations.

- **Graphs** – nodes (vertices) connected by edges; can be directed or undirected, weighted or unweighted.  Represented via adjacency lists or matrices.
## Common algorithmic patterns

- **Two‑pointers / sliding window** – maintain two indices moving through an array to find subarrays or pairs meeting certain criteria (e.g., longest substring without repeating characters).

- **Divide and conquer** – split a problem into sub‑problems, solve them recursively and combine results (e.g., mergesort, quicksort).

- **Dynamic programming** – break down problems into overlapping subproblems and store intermediate results to avoid recomputation (e.g., Fibonacci, knapsack, longest common subsequence).

- **Greedy algorithms** – make locally optimal choices in hopes of finding a global optimum (e.g., Dijkstra’s algorithm for shortest paths, interval scheduling).

- **Backtracking** – try out possible solutions and abandon those that violate constraints (e.g., N‑Queens, permutations).
## Interview tips

1. Clarify the problem and constraints before coding.
2. Start with a brute‑force solution then look for optimizations.
3. Consider edge cases (empty input, large numbers, duplicates).
4. Use proper data structures – for example, a `Dictionary<TKey,TValue>` in C# for O(1) lookups, or a `PriorityQueue` for scheduling.
5. Communicate your thought process; interviewers want to see how you approach problems.

These fundamentals will help you tackle a wide variety of algorithmic questions.