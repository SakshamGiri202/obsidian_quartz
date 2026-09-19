Python 2 Data Structures & Algorithms Roadmap (Assuming basic Python 2 knowledge)

| Week | Topic                          | Sub‑topics                                                                                 | Time Estimate |
| ---- | ------------------------------ | ------------------------------------------------------------------------------------------ | ------------- |
| 1    | **Review & Setup**             | Python 2 syntax refresher, basic I/O, installing pip/virtualenv, running scripts           | 4 h           |
| 2    | **Complexity Analysis**        | Big‑O, time/space, amortized analysis, best/average/worst case                             | 3 h           |
| 3    | **Arrays & Lists**             | List operations, slicing, list comprehensions, 2‑D lists, performance pitfalls             | 4 h           |
| 4    | **Strings**                    | Immutability, common methods, formatting, regex basics, Unicode handling in Py2            | 3 h           |
| 5    | **Tuples & Sets**              | Tuple unpacking, frozenset, set algebra, hashability                                       | 2 h           |
| 6    | **Dictionaries & Hash Tables** | Dict internals, hashing, collision handling, defaultdict, Counter                          | 4 h           |
| 7    | **Stacks**                     | List‑based stack, push/pop, applications (parentheses matching, DFS)                       | 2 h           |
| 8    | **Queues**                     | collections.deque, circular buffer, priority queue via heapq, BFS                          | 3 h           |
| 9    | **Linked Lists**               | Singly & doubly linked nodes, insertion/deletion, reversing, cycle detection               | 4 h           |
| 10   | **Recursion**                  | Base case, tail recursion, recursion vs iteration, memoization intro                       | 3 h           |
| 11   | **Sorting Algorithms**         | Bubble, insertion, selection (O(n²)), merge sort, quicksort, heap sort, timsort (Py2 sort) | 6 h           |
| 12   | **Searching**                  | Linear search, binary search (iterative & recursive), interpolation search                 | 3 h           |
| 13   | **Trees**                      | Binary tree traversals (pre/in/post/level), BST properties, AVL basics, heap property      | 5 h           |
| 14   | **Graphs**                     | Representation (adj list/matrix), DFS, BFS, shortest path (Dijkstra), topological sort     | 5 h           |
| 15   | **Dynamic Programming**        | Memoization vs tabulation, classic problems (fib, knapsack, LCS, coin change)              | 6 h           |
| 16   | **Greedy Algorithms**          | Activity selection, Huffman coding, fractional knapsack, interval scheduling               | 3 h           |
| 17   | **Backtracking**               | N‑Queens, Sudoku solver, subset sum, permutation generation                                | 4 h           |
| 18   | **Advanced Topics**            | Trie, segment tree, Fenwick tree, disjoint set (union‑find), basic string matching (KMP)   | 6 h           |
| 19   | **Practice & Review**          | Solve 2‑3 problems per topic on platforms (LeetCode Easy/Medium, HackerRank)               | 8 h           |
| 20   | **Capstone Project**           | Implement a small utility (e.g., spell checker, maze solver) using multiple DS/AL          | 6 h           |

**Total Estimated Time:** ~80 hours (≈2 hours/day for 6 weeks). Adjust based on your pace; focus on understanding concepts and coding each structure/algorithm at least once.



---
title: DSA Roadmap (Python) - Practical & Hands-on
tags: [dsa, python, interview-prep, hands-on]
created: 2026-09-17

---


# 🚀 DSA Roadmap (Python)

## 📌 Hands-On Execution Strategy (No Video Tutorials)

**Total Estimated Time:** ~80 hours (≈2 hours/day for 6 weeks). Adjust based on your pace; focus on understanding concepts and coding each structure/algorithm at least once.

- [ ] **Read & Implement First:** For every topic, read official docs, Python typing annotations, or text-based articles (like Real Python, GeeksforGeeks, or LeetCode editorial articles) instead of watching video tutorials.
- [ ] **Manual Implementation:** Write every data structure and algorithm from scratch using pure Python classes before using Python’s built-in modules (`collections`, `heapq`, `bisect`, etc.).

- [ ] **Interactive Debugging:** Use Python’s built-in `pdb` module, `print()` debugging, or Python Tutor (online visualizer) to trace execution flow line-by-line manually.
- [ ] **Solve 5–10 Problems per Topic:** Move to problem sets only after your custom implementation passes basic test cases.
- [ ] **Big-O Analysis:** Write down Time & Space Complexity annotations in docstrings for every function you write.

---

## 1️⃣ Arrays & Lists

### Basic Operations (Built-in `list`)

- [ ] Creation & List Comprehensions (`[0] * n`, `[x for x in range(n)]`)
- [ ] Indexing & Slicing (`lst[a:b:step]`) — O(k) step slice copy
- [ ] Traversal (`for item in lst:`, `enumerate()`, `zip()`)
- [ ] Insert/Delete at end (`append`, `pop`) — O(1) amortized
- [ ] Insert/Delete at arbitrary index (`insert`, `pop(i)`, `del`) — O(n)
- [ ] Linear Search (`in` keyword, `index()`) — O(n)
- [ ] In-place modification vs returning a copy (`sort()` vs `sorted()`)

### Intermediate Concepts & Python Internals

- [ ] Python `list` internal structure (dynamic array allocation, pointer arrays)
- [ ] Dynamic resizing & over-allocation mechanics
- [ ] Multi-dimensional lists / 2D Grids (`[[0]*col for _ in range(row)]`)
- [ ] Shallow copy (`copy()`, `[:]`) vs Deep copy (`copy.deepcopy()`)

### Core Patterns (Implement from scratch!)

- [ ] Two Pointers (opposite directions & fast/slow)
- [ ] Sliding Window (fixed-size & dynamic variable size)
- [ ] Prefix Sum & Difference Arrays
- [ ] Kadane's Algorithm (maximum subarray sum)
- [ ] Dutch National Flag Algorithm (3-way partitioning)
- [ ] Binary Search on Arrays (`bisect_left`, `bisect_right` implementation)
- [ ] Interval Merging & Overlap checks
- [ ] Cyclic Sort

### Practice Problems

Two Sum, Best Time to Buy/Sell Stock, Maximum Subarray, Move Zeroes, Product of Array Except Self, Rotate Array, Merge Sorted Array, Container With Most Water, Trapping Rain Water, 3Sum, Subarray Sum Equals K.

---

## 2️⃣ Strings

_(Sequences of immutable Unicode characters in Python)_

- [ ] String immutability in Python & string concatenation cost (`+` vs `''.join()`)
- [ ] Traversal, slicing, and ASCII/Unicode operations (`ord()`, `chr()`)
- [ ] Built-in utility methods (`split`, `join`, `strip`, `replace`, `find`, `startswith`, `endswith`)
- [ ] String formatting & f-strings
- [ ] In-place string pattern simulation using `list(string)`
- [ ] Palindrome checks (two-pointer vs slicing)
- [ ] Anagram checks (Frequency arrays vs `collections.Counter`)
- [ ] Pattern Matching basics (Naive, Rabin-Karp, KMP algorithm)

### Practice Problems

Valid Anagram, Longest Palindromic Substring, Longest Substring Without Repeating Characters, Group Anagrams, Valid Palindrome, Minimum Window Substring.

---

## 3️⃣ Stack (LIFO)

### Basic Operations

- [ ] Implement using Python `list` (`append()` and `pop()`)
- [ ] Implement using `collections.deque` (thread-safe, efficient double-ended queue)
- [ ] Build a custom `Stack` class with methods: `push()`, `pop()`, `peek()`, `is_empty()`, `size()`
- [ ] Implement using Linked List (pointer-based stack)

### Intermediate Concepts

- [ ] Min Stack / Max Stack (tracking extremes in O(1) time using auxiliary stack)
- [ ] Monotonic Stack (Next Greater Element, Next Smaller Element)
- [ ] Expression Parsing: Infix to Postfix/Prefix conversion & evaluation
- [ ] Call Stack simulation & explicit stack recursion unwinding

### Practice Problems

Valid Parentheses, Min Stack, Daily Temperatures, Largest Rectangle in Histogram, Evaluate Reverse Polish Notation, Asteroid Collision, Online Stock Span.

---

## 4️⃣ Queue (FIFO) & Deque

### Basic Operations

- [ ] Explain why Python `list.pop(0)` is O(n) and inefficient for queues
- [ ] Implement Queue using `collections.deque` (`append()` and `popleft()`)
- [ ] Build a custom `Queue` class from scratch using Linked List nodes
- [ ] Implement `CircularQueue` with fixed array size and pointers (`head`, `tail`)
- [ ] Implement Double-Ended Queue (`Deque`) manually

### Intermediate Concepts

- [ ] Monotonic Queue (Sliding Window Maximum)
- [ ] Queue using two Stacks
- [ ] Stack using two Queues

### Practice Problems

Implement Queue using Stacks, Design Circular Queue, Sliding Window Maximum, Design Hit Counter, Task Scheduler.

---

## 5️⃣ Linked List

### Basic Operations

- [ ] Node class implementation (`class Node: def __init__(self, val=0, next=None):`)
- [ ] Singly Linked List: Insertion & Deletion at Head, Tail, and Middle
- [ ] Traversal, Search, and Length calculation
- [ ] Reverse Linked List (Iterative & Recursive approaches)

### Intermediate Concepts

- [ ] Doubly Linked List node (`prev`, `val`, `next`) implementation
- [ ] Circular Linked List
- [ ] Fast & Slow Pointers (Floyd’s Cycle Finding Algorithm)
- [ ] Find Middle Node
- [ ] Merge Two Sorted Lists
- [ ] Remove N-th node from end (single-pass pointer difference)
- [ ] Palindrome Linked List
- [ ] LRU Cache (Doubly Linked List + Python `dict`)

### Practice Problems

Reverse Linked List, Linked List Cycle, Merge Two Sorted Lists, Remove Nth Node From End, Reorder List, Add Two Numbers, Copy List with Random Pointer, LRU Cache.

---

## 🔀 Checkpoint: Linear Data Structures

Before advancing to non-linear structures, solve 3 random problems combining Lists, Stacks, Queues, and Linked Lists without checking solutions.

---

## 6️⃣ Hash Map & Hash Set

### Basic Operations

- [ ] Python built-in `dict` and `set` mechanics (hash tables under the hood)
- [ ] Special Python dictionaries: `collections.defaultdict`, `collections.Counter`, `collections.OrderedDict`
- [ ] Hash functions & key immutability requirements (tuples/strings vs lists/dicts)

### Intermediate Concepts (Build from scratch)

- [ ] Build a custom `HashTable` class using fixed-size list of buckets
- [ ] Collision resolution techniques: Separate Chaining (Linked Lists) vs Open Addressing (Linear Probing)
- [ ] Load Factor calculation & Dynamic Rehashing (doubling capacity)
- [ ] Frequency counting & grouping patterns
- [ ] Subarray sum problems using Prefix Sum + Hash Map

### Practice Problems

Two Sum, Group Anagrams, Subarray Sum Equals K, Longest Consecutive Sequence, Top K Frequent Elements, Contains Duplicate, Design HashMap, 4Sum II.

---

## 7️⃣ Trees & Binary Search Trees (BST)

### Basic Operations (Binary Tree)

- [ ] Node class (`class TreeNode: def __init__(self, val=0, left=None, right=None):`)
- [ ] Recursive Traversals: Preorder, Inorder, Postorder
- [ ] Iterative Traversals (using explicit stack)
- [ ] Level Order Traversal / BFS (using `collections.deque`)
- [ ] Calculate Depth, Height, Node Count, Leaf Count

### Binary Search Tree (BST)

- [ ] BST Property: `left < root < right`
- [ ] Search, Insert, and Delete (handling 0, 1, and 2 children cases)
- [ ] Lowest Common Ancestor (LCA) in BST
- [ ] Convert Sorted Array to Balanced BST
- [ ] Validate BST (min/max range tracking)

### Intermediate & Advanced Trees

- [ ] Trie (Prefix Tree): Build `TrieNode` with dict of children & `is_end_of_word` flag
- [ ] Balanced BST concepts (AVL Tree rotation rules, Red-Black Tree theory)
- [ ] Segment Tree (Range sum/min queries)
- [ ] Binary Indexed Tree (Fenwick Tree)
- [ ] Lowest Common Ancestor (General Binary Tree)
- [ ] Tree Serialization & Deserialization

### Practice Problems

Maximum Depth of Binary Tree, Validate BST, Binary Tree Level Order Traversal, Lowest Common Ancestor of BST, Diameter of Binary Tree, Implement Trie, Kth Smallest Element in a BST, Serialize and Deserialize Binary Tree.

---

## 8️⃣ Heap & Priority Queue

### Basic Operations

- [ ] Array-based binary heap indexing: Parent `(i-1)//2`, Left Child `2i+1`, Right Child `2i+2`
- [ ] Build a `MinHeap` and `MaxHeap` class from scratch:
  - [ ] `sift_up()` / `heapify_up()` — O(log n)
  - [ ] `sift_down()` / `heapify_down()` — O(log n)
  - [ ] `insert()` and `extract_min()` — O(log n)
  - [ ] `heapify()` (build heap from unsorted array in O(n) time)
- [ ] Python’s built-in `heapq` module (`heappush`, `heappop`, `heapify`, `heappushpop`, `nlargest`, `nsmallest`)
- [ ] Handling Max-Heaps in Python (negating numbers technique)

### Intermediate Concepts

- [ ] Heap Sort implementation
- [ ] Top K Elements pattern
- [ ] Two-Heap pattern (Find Median from Data Stream)
- [ ] Custom objects/tuples in `heapq` with custom comparator wrappers

### Practice Problems

Kth Largest Element in an Array, Merge k Sorted Lists, Top K Frequent Elements, Find Median from Data Stream, Task Scheduler, Meeting Rooms II.

---

## 9️⃣ Graphs

### Representation Techniques

- [ ] Adjacency List (using `dict` or `defaultdict(list)`)
- [ ] Adjacency Matrix (2D list)
- [ ] Edge List
- [ ] Grid as an implicit graph

### Fundamental Traversals & Algorithms

- [ ] Breadth-First Search (BFS) using Queue
- [ ] Depth-First Search (DFS) using Recursion and Iterative Stack
- [ ] Connected Components counting
- [ ] Cycle Detection (Undirected graph using BFS/DFS; Directed graph using 3-color marking)
- [ ] Topological Sorting (Kahn’s Algorithm / Indegree BFS & DFS post-order)
- [ ] Union-Find / Disjoint Set Union (DSU) with Path Compression and Union by Rank

### Advanced Graph Algorithms

- [ ] Dijkstra’s Algorithm (Shortest path in weighted graph using `heapq`)
- [ ] Bellman-Ford Algorithm (Handles negative edge weights & detects negative cycles)
- [ ] Floyd-Warshall Algorithm (All-pairs shortest path)
- [ ] Prim’s & Kruskal’s Algorithms (Minimum Spanning Tree - MST)
- [ ] Bipartite Graph Verification
- [ ] Strongly Connected Components (Tarjan’s / Kosaraju’s Algorithm)

### Practice Problems

Number of Islands, Clone Graph, Course Schedule (I & II), Rotting Oranges, Word Ladder, Network Delay Time, Redundant Connection, Graph Valid Tree, Pacific Atlantic Water Flow.

---

## 🔟 Recursion & Backtracking

### Fundamentals

- [ ] Base case, Recursive relation, and Recursion Tree sketching
- [ ] Python call stack depth limit (`sys.getrecursionlimit()`, `sys.setrecursionlimit()`)
- [ ] State space tree visualization
- [ ] Standard Backtracking Template: `Choose -> Explore -> Unchoose`

### Key Patterns

- [ ] Generating Subsets (Power Set)
- [ ] Generating Permutations & Combinations
- [ ] Grid Backtracking (Matrix traversal)
- [ ] Constraint Satisfaction Problems (N-Queens, Sudoku Solver)

### Practice Problems

Subsets, Permutations, Combination Sum, N-Queens, Letter Combinations of a Phone Number, Word Search, Palindrome Partitioning.

---

## 1️⃣1️⃣ Sorting & Searching

### Sorting Algorithms (Implement manually, analyze stability & complexity)

- [ ] Bubble Sort — O(n²)
- [ ] Selection Sort — O(n²)
- [ ] Insertion Sort — O(n²), efficient for small/nearly-sorted inputs
- [ ] Merge Sort — O(n log n), Divide & Conquer, Stable
- [ ] Quick Sort — O(n log n) average, partition schemes (Lomuto vs Hoare)
- [ ] Heap Sort — O(n log n)
- [ ] Counting Sort / Radix Sort (Non-comparison based) — O(n + k)

### Searching Algorithms & Binary Search Patterns

- [ ] Linear Search — O(n)
- [ ] Standard Binary Search (Iterative & Recursive) — O(log n)
- [ ] Binary Search on Answer Space (Minimizing/Maximizing feasible constraints)
- [ ] Python’s built-in `bisect` module (`bisect_left`, `bisect_right`)

### Practice Problems

Binary Search, Search in Rotated Sorted Array, Find First and Last Position of Element in Sorted Array, Kth Largest Element, Sort Colors, Capacity To Ship Packages Within D Days.

---

## 1️⃣2️⃣ Dynamic Programming (DP)

### Foundations

- [ ] Overlapping Subproblems & Optimal Substructure
- [ ] Top-Down Approach (Recursion + Memoization) using Python's `@functools.lru_cache` or manual `memo` dictionary
- [ ] Bottom-Up Approach (Iterative Tabulation)
- [ ] Space Optimization techniques (reducing 2D DP array to 1D array)

### Essential DP Patterns

- [ ] 1D DP: Climbing Stairs, House Robber, Coin Change
- [ ] Unbounded & 0/1 Knapsack Patterns
- [ ] Longest Common Subsequence (LCS) & String Alignment
- [ ] Longest Increasing Subsequence (LIS) — O(n²) and O(n log n) with binary search
- [ ] Matrix / Grid DP (Unique Paths, Minimum Path Sum)
- [ ] Interval DP & DP on Palindromes
- [ ] Bitmask DP (Advanced)
- [ ] Tree DP (Advanced)

### Practice Problems

Climbing Stairs, House Robber, Coin Change, Longest Common Subsequence, Longest Increasing Subsequence, Edit Distance, 0/1 Knapsack, Partition Equal Subset Sum, Word Break, Unique Paths, Target Sum.

---

## 1️⃣3️⃣ Greedy Algorithms

- [ ] Understanding Greedy Choice Property vs Dynamic Programming
- [ ] Interval Scheduling / Activity Selection
- [ ] Fractional Knapsack
- [ ] Greedy Choice Verification & Proof of Correctness

### Practice Problems

Jump Game (I & II), Gas Station, Meeting Rooms, Task Scheduler, Minimum Number of Arrows to Burst Balloons, Partition Labels.

---

## 1️⃣4️⃣ Bit Manipulation

- [ ] Python Integer Representation (arbitrary precision integers)
- [ ] Bitwise Operators: AND (`&`), OR (`|`), XOR (`^`), NOT (`~`), Left Shift (`<<`), Right Shift (`>>`)
- [ ] Common Operations:
  - Check if $i$-th bit is set: `(num & (1 << i)) != 0`
  - Set $i$-th bit: `num | (1 << i)`
  - Clear $i$-th bit: `num & ~(1 << i)`
  - Toggle $i$-th bit: `num ^ (1 << i)`
  - Clear lowest set bit: `num & (num - 1)`
  - Is power of two check: `num > 0 and (num & (num - 1)) == 0`
- [ ] Bitmasking subsets generator

### Practice Problems

Single Number, Number of 1 Bits, Counting Bits, Reverse Bits, Power of Two, Missing Number, Subsets (using Bitmask).

---

## 🧩 Python DSA Complexity & Method Reference

| Data Structure / Operation | Access | Search | Insert | Delete | Python Built-In Equivalents |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Array / List** | $O(1)$ | $O(n)$ | $O(n)$ end: $O(1)$ | $O(n)$ end: $O(1)$ | `list` |
| **Stack** | $O(n)$ | $O(n)$ | $O(1)$ | $O(1)$ | `collections.deque` / `list` |
| **Queue** | $O(n)$ | $O(n)$ | $O(1)$ | $O(1)$ | `collections.deque` |
| **Singly Linked List** | $O(n)$ | $O(n)$ | $O(1)^*$ | $O(1)^*$ | Custom Class |
| **Hash Map / Set** | — | $O(1)$ avg | $O(1)$ avg | $O(1)$ avg | `dict`, `set`, `defaultdict` |
| **BST (Balanced)** | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ | Custom Class |
| **Heap / Priority Queue** | $O(1)$ top | $O(n)$ | $O(\log n)$ | $O(\log n)$ | `heapq` module |
| **Graph (Adjacency List)**| — | $O(V+E)$ | $O(1)$ | $O(V+E)$ | `dict` of `lists` / `sets` |

*\*Requires existing direct reference to target node position.*

---

## ✅ Progress Tracker

- [ ] Arrays & Lists
- [ ] Strings
- [ ] Stack
- [ ] Queue & Deque
- [ ] Linked List
- [ ] Hash Map & Set
- [ ] Trees & BST
- [ ] Trie
- [ ] Heap & Priority Queue
- [ ] Graphs & DSU
- [ ] Recursion & Backtracking
- [ ] Sorting & Searching
- [ ] Dynamic Programming
- [ ] Greedy Algorithms
- [ ] Bit Manipulation
