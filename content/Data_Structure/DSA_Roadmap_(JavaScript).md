


---

title: DSA Roadmap (JavaScript)

tags: [dsa, javascript, interview-prep]

created: 2026-08-21

---


# DSA Roadmap (JavaScript)

## 📌 How to Use This Doc

- [ ] Check off each topic as you learn it
- [ ] Implement every "from scratch" operation manually at least once (don't just use built-ins)
- [ ] Solve 5–10 problems per topic before moving on
- [ ] Revisit Big-O for every operation you learn

---

## 1️⃣ Arrays

### Basic Operations

- [ ] Declaration & initialization (`[]`, `new Array(n)`, `Array.from`)
- [ ] Access by index — O(1)
- [ ] Traversal (`for`, `for...of`, `forEach`)
- [ ] Insert at end (`push`) — O(1) amortized
- [ ] Insert at beginning (`unshift`) — O(n)
- [ ] Delete from end (`pop`) — O(1)
- [ ] Delete from beginning (`shift`) — O(n)
- [ ] Insert/delete at arbitrary index (`splice`) — O(n)
- [ ] Search (linear search) — O(n)
- [ ] Update value at index — O(1)

### Intermediate Operations

- [ ] `slice()` vs `splice()` (immutable vs mutable)
- [ ] `map`, `filter`, `reduce`, `find`, `findIndex`, `some`, `every`
- [ ] `sort()` (default lexicographic vs comparator) — O(n log n)
- [ ] `reverse()`
- [ ] 2D arrays / matrices (creation, traversal, rotation, transpose)
- [ ] Multi-dimensional array manipulation
- [ ] Array destructuring & spread operator
- [ ] `flat()` / `flatMap()`

### Core Patterns (practice these!)

- [ ] Two Pointers (opposite ends / same direction)
- [ ] Sliding Window (fixed & variable size)
- [ ] Prefix Sum / Suffix Sum
- [ ] Kadane's Algorithm (max subarray)
- [ ] Dutch National Flag (sort 0s,1s,2s)
- [ ] Binary Search on Arrays (and "search in rotated sorted array")
- [ ] Merge Intervals
- [ ] Cyclic Sort

### Practice Problems

Two Sum, Best Time to Buy/Sell Stock, Max Subarray, Move Zeroes, Product of Array Except Self, Rotate Array, Merge Sorted Array, Container With Most Water, Trapping Rain Water, 3Sum, Subarray Sum Equals K

---

## 2️⃣ Strings

_(treated as a special linear array — study right after arrays)_

- [ ] String immutability in JS
- [ ] Traversal, indexing, `charAt`, `charCodeAt`
- [ ] `split`, `join`, `slice`, `substring`, `substr`
- [ ] `includes`, `indexOf`, `startsWith`, `endsWith`
- [ ] Reverse a string (multiple ways)
- [ ] Palindrome check
- [ ] Anagram check (frequency map)
- [ ] String Sliding Window (Longest Substring Without Repeating Characters)
- [ ] Pattern Matching basics (naive, then KMP / Rabin-Karp — advanced)

### Practice Problems

Valid Anagram, Longest Palindromic Substring, Longest Substring Without Repeating Characters, Group Anagrams, Valid Parentheses (bridges into Stack topic below)

---

## 3️⃣ Stack (LIFO) — Linear, Static/Dynamic

### Basic Operations

- [ ] Implement using Array (`push`/`pop` as `push`/`pop`)
- [ ] Implement using Linked List (for true O(1) without resizing concerns)
- [ ] `push` — O(1)
- [ ] `pop` — O(1)
- [ ] `peek/top` — O(1)
- [ ] `isEmpty` — O(1)
- [ ] `size` — O(1)

### Intermediate Concepts

- [ ] Min Stack (track minimum in O(1))
- [ ] Balanced Parentheses / Valid Brackets
- [ ] Infix → Postfix / Prefix conversion
- [ ] Evaluate Postfix / Prefix expressions
- [ ] Next Greater Element (Monotonic Stack)
- [ ] Next Smaller Element
- [ ] Stock Span Problem
- [ ] Implement Queue using two Stacks
- [ ] Call stack & recursion relationship (important conceptual link)

### Practice Problems

Valid Parentheses, Min Stack, Daily Temperatures, Largest Rectangle in Histogram, Evaluate Reverse Polish Notation, Next Greater Element I/II, Asteroid Collision

---

## 4️⃣ Queue (FIFO) — Linear

### Basic Operations

- [ ] Implement using Array (note: `shift()` is O(n) — discuss trade-off)
- [ ] Implement using Linked List (O(1) enqueue/dequeue)
- [ ] `enqueue` — O(1)
- [ ] `dequeue` — O(1) with LL, O(n) with naive array
- [ ] `front/peek` — O(1)
- [ ] `isEmpty`, `size`

### Intermediate Concepts

- [ ] Circular Queue (fixed-size buffer)
- [ ] Deque (Double-Ended Queue) — insert/remove both ends
- [ ] Priority Queue (intro — full version needs Heap, covered later)
- [ ] Monotonic Queue (Sliding Window Maximum)
- [ ] Implement Stack using two Queues

### Practice Problems

Implement Queue using Stacks, Circular Queue design, Sliding Window Maximum, Design Hit Counter, Task Scheduler

---

## 5️⃣ Linked List — Linear, Dynamic

### Basic Operations

- [ ] Node structure (`{ value, next }`)
- [ ] Singly Linked List: insert at head/tail/middle
- [ ] Delete at head/tail/middle
- [ ] Traversal & search — O(n)
- [ ] Get length
- [ ] Reverse a linked list (iterative & recursive)

### Intermediate Concepts

- [ ] Doubly Linked List (insert/delete both directions)
- [ ] Circular Linked List
- [ ] Fast & Slow Pointers (Floyd's Cycle Detection)
- [ ] Detect & find start of cycle
- [ ] Find middle of linked list
- [ ] Merge two sorted linked lists
- [ ] Remove Nth node from end
- [ ] Palindrome Linked List
- [ ] Flatten a multilevel linked list
- [ ] LRU Cache (Doubly Linked List + Hash Map) — bridges into Hash Map topic

### Practice Problems

Reverse Linked List, Linked List Cycle, Merge Two Sorted Lists, Remove Nth Node From End, Reorder List, Add Two Numbers, Copy List with Random Pointer, LRU Cache

---

## 🔀 Checkpoint: Linear DS Complete

Before moving on, be comfortable converting between array ↔ stack ↔ queue ↔ linked list implementations, and know when each is the right tool.

---

## 6️⃣ Hash Map / Hash Set — Non-Linear

### Basic Operations

- [ ] JS built-ins: `Map`, `Set`, plain `Object` as hash map
- [ ] Insert / update — O(1) average
- [ ] Lookup — O(1) average
- [ ] Delete — O(1) average
- [ ] Iteration order (Map preserves insertion order, Object mostly does too for string keys)
- [ ] `WeakMap` / `WeakSet` (conceptual — memory management)

### Intermediate Concepts

- [ ] Build your own hash map from scratch (hash function + array of buckets)
- [ ] Collision handling: chaining vs open addressing
- [ ] Load factor & resizing/rehashing
- [ ] Frequency counting pattern
- [ ] Two Sum with Hash Map (O(n) vs O(n²) brute force)
- [ ] Grouping pattern (group anagrams, group by key)
- [ ] Hash Set for deduplication / existence checks
- [ ] Sliding window + hash map (longest substring problems)

### Practice Problems

Two Sum, Group Anagrams, Subarray Sum Equals K, Longest Consecutive Sequence, Top K Frequent Elements, Contains Duplicate, Design HashMap (build from scratch), 4Sum II

---

## 7️⃣ Trees — Non-Linear

### Basic Operations (Binary Tree)

- [ ] Node structure (`{ value, left, right }`)
- [ ] Insert, search
- [ ] Traversals: Preorder, Inorder, Postorder (recursive & iterative)
- [ ] Level Order Traversal (BFS using Queue)
- [ ] Height / Depth of tree
- [ ] Count nodes / leaves

### Binary Search Tree (BST)

- [ ] Insert — O(log n) avg / O(n) worst
- [ ] Search — O(log n) avg
- [ ] Delete (3 cases: leaf, one child, two children)
- [ ] Validate BST
- [ ] Find min/max
- [ ] Successor/Predecessor
- [ ] Convert sorted array to balanced BST
- [ ] Kth smallest/largest element

### Intermediate/Advanced Trees

- [ ] Balanced Trees concept: AVL, Red-Black (theory level is enough for most interviews)
- [ ] Trie (Prefix Tree): insert, search, startsWith
- [ ] Segment Tree (range queries) — intermediate-advanced
- [ ] Fenwick Tree / Binary Indexed Tree — advanced
- [ ] N-ary Tree traversal
- [ ] Lowest Common Ancestor (LCA)
- [ ] Diameter of Binary Tree
- [ ] Serialize/Deserialize a tree

### Practice Problems

Max Depth of Binary Tree, Validate BST, Level Order Traversal, LCA of BST, Diameter of Binary Tree, Implement Trie, Word Search II, Kth Smallest Element in BST, Serialize/Deserialize Binary Tree

---

## 8️⃣ Heap / Priority Queue — Non-Linear

### Basic Operations

- [ ] Array-based representation (parent = `(i-1)/2`, children = `2i+1`,`2i+2`)
- [ ] Min-Heap vs Max-Heap
- [ ] `insert` (heapify up) — O(log n)
- [ ] `extractMin/Max` (heapify down) — O(log n)
- [ ] `peek` — O(1)
- [ ] Build Heap from array — O(n)
- [ ] JS has no built-in heap — implement manually

### Intermediate Concepts

- [ ] Heap Sort
- [ ] Kth Largest/Smallest Element
- [ ] Top K Frequent Elements (heap-based)
- [ ] Merge K Sorted Lists
- [ ] Median Finder (two heaps)
- [ ] Custom priority via comparator function

### Practice Problems

Kth Largest Element in Array, Merge K Sorted Lists, Top K Frequent Elements, Find Median from Data Stream, Task Scheduler, Meeting Rooms II

---

## 9️⃣ Graphs — Non-Linear

### Basic Concepts & Representation

- [ ] Terminology: vertex, edge, directed/undirected, weighted/unweighted, cyclic/acyclic
- [ ] Adjacency Matrix
- [ ] Adjacency List (most common in JS — `Map<node, array>`)
- [ ] Build a graph from edge list

### Basic Traversals

- [ ] BFS (using Queue) — O(V+E)
- [ ] DFS (recursive & iterative using Stack) — O(V+E)
- [ ] Connected Components

### Intermediate Algorithms

- [ ] Cycle Detection (directed — using colors/states; undirected — using DFS/Union-Find)
- [ ] Topological Sort (Kahn's Algorithm / DFS-based)
- [ ] Union-Find / Disjoint Set (with path compression + union by rank)
- [ ] Number of Islands (grid as graph, BFS/DFS)
- [ ] Bipartite Graph Check
- [ ] Clone Graph

### Advanced Algorithms

- [ ] Dijkstra's Algorithm (shortest path, weighted, non-negative) — uses Heap
- [ ] Bellman-Ford (handles negative weights)
- [ ] Floyd-Warshall (all-pairs shortest path)
- [ ] Prim's & Kruskal's (Minimum Spanning Tree)
- [ ] A* Search (conceptual)
- [ ] Strongly Connected Components (Tarjan's / Kosaraju's) — advanced/optional

### Practice Problems

Number of Islands, Clone Graph, Course Schedule (I & II), Rotting Oranges, Word Ladder, Network Delay Time, Redundant Connection, Graph Valid Tree, Pacific Atlantic Water Flow

---

## 🔟 Recursion & Backtracking

- [ ] Base case + recursive case fundamentals
- [ ] Recursion tree & call stack visualization
- [ ] Memoization intro (bridges into DP)
- [ ] Backtracking template: choose → explore → un-choose
- [ ] Permutations & Combinations
- [ ] Subsets (power set)
- [ ] N-Queens
- [ ] Sudoku Solver
- [ ] Word Search (grid backtracking)
- [ ] Combination Sum

### Practice Problems

Subsets, Permutations, Combination Sum, N-Queens, Letter Combinations of Phone Number, Word Search, Palindrome Partitioning

---

## 1️⃣1️⃣ Sorting & Searching

### Sorting (know time/space complexity + stability for each)

- [ ] Bubble Sort — O(n²)
- [ ] Selection Sort — O(n²)
- [ ] Insertion Sort — O(n²), good for nearly-sorted data
- [ ] Merge Sort — O(n log n), stable
- [ ] Quick Sort — O(n log n) avg, O(n²) worst
- [ ] Heap Sort — O(n log n)
- [ ] Counting Sort / Radix Sort (non-comparison, O(n+k)) — intermediate

### Searching

- [ ] Linear Search — O(n)
- [ ] Binary Search (iterative & recursive) — O(log n)
- [ ] Binary Search variants: first/last occurrence, search in rotated sorted array, search in 2D matrix
- [ ] Ternary Search (optional/advanced)

### Practice Problems

Binary Search, Search in Rotated Sorted Array, Find First and Last Position, Kth Largest Element, Merge Sort implementation, Quick Sort implementation, Sort Colors

---

## 1️⃣2️⃣ Dynamic Programming (DP)

### Foundations

- [ ] Overlapping subproblems + optimal substructure
- [ ] Memoization (Top-Down) vs Tabulation (Bottom-Up)
- [ ] Convert recursive brute force → memoized → tabulated
- [ ] 1D DP (Fibonacci, Climbing Stairs, House Robber)
- [ ] 2D DP (Grid paths, Edit Distance, LCS)

### Classic DP Patterns

- [ ] Knapsack (0/1 Knapsack, Unbounded Knapsack)
- [ ] Longest Common Subsequence (LCS)
- [ ] Longest Increasing Subsequence (LIS)
- [ ] Edit Distance
- [ ] Coin Change (min coins & count ways)
- [ ] Partition Equal Subset Sum
- [ ] Matrix Chain Multiplication (advanced)
- [ ] DP on Strings (Palindromic Substrings, Distinct Subsequences)
- [ ] DP on Trees (advanced/optional)
- [ ] DP with Bitmasking (advanced/optional)

### Practice Problems

Climbing Stairs, House Robber, Coin Change, LCS, LIS, Edit Distance, 0/1 Knapsack, Partition Equal Subset Sum, Word Break, Unique Paths, Maximum Product Subarray

---

## 1️⃣3️⃣ Greedy Algorithms

- [ ] Greedy choice property vs DP (when greedy fails)
- [ ] Activity Selection
- [ ] Fractional Knapsack
- [ ] Jump Game (I & II)
- [ ] Gas Station
- [ ] Meeting Rooms
- [ ] Huffman Encoding (conceptual)

### Practice Problems

Jump Game, Gas Station, Meeting Rooms, Task Scheduler, Minimum Number of Arrows to Burst Balloons

---

## 1️⃣4️⃣ Bit Manipulation (supporting topic)

- [ ] AND, OR, XOR, NOT, shifts (`<<`, `>>`, `>>>`)
- [ ] Check/set/clear a bit
- [ ] Count set bits (Brian Kernighan's algorithm)
- [ ] Power of Two check
- [ ] Single Number (XOR trick)
- [ ] Bitmasking for subsets (used in advanced DP)

### Practice Problems

Single Number, Number of 1 Bits, Counting Bits, Power of Two, Sum of Two Integers (without +/-)

---

## 📈 Suggested Study Order (Summary)

1. Arrays → Strings
2. Stack → Queue
3. Linked List
4. Recursion basics (needed before Trees/Backtracking)
5. Hash Map / Hash Set
6. Trees → BST → Trie
7. Heap / Priority Queue
8. Graphs (BFS/DFS → Union-Find → Topo Sort → Dijkstra)
9. Sorting & Searching (can be interleaved earlier)
10. Backtracking
11. Dynamic Programming
12. Greedy
13. Bit Manipulation (interleave anytime)

---

## 🧩 Complexity Cheat Sheet

|Structure|Access|Search|Insert|Delete|
|---|---|---|---|---|
|Array|O(1)|O(n)|O(n)|O(n)|
|Stack|O(n)|O(n)|O(1)|O(1)|
|Queue|O(n)|O(n)|O(1)|O(1)|
|Linked List|O(n)|O(n)|O(1)*|O(1)*|
|Hash Map|—|O(1) avg|O(1) avg|O(1) avg|
|BST (balanced)|O(log n)|O(log n)|O(log n)|O(log n)|
|Heap|O(1) top|O(n)|O(log n)|O(log n)|
|Graph (adj list)|—|O(V+E)|O(1)|O(V+E)|

*O(1) if you already have a reference to the node/position.

---

## ✅ Tracker

- [ ] Arrays
- [ ] Strings
- [ ] Stack
- [ ] Queue
- [ ] Linked List
- [ ] Hash Map/Set
- [ ] Trees & BST
- [ ] Trie
- [ ] Heap
- [ ] Graphs
- [ ] Recursion & Backtracking
- [ ] Sorting & Searching
- [ ] Dynamic Programming
- [ ] Greedy
- [ ] Bit Manipulation