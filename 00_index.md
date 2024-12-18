# Data Structures and Algorithms (DSA)

# Complexity Analysis

## Time Complexity

Time complexity measures how the runtime of an algorithm grows with the input size.

| **Complexity** | **Description**                         |
|----------------|-----------------------------------------|
| **O(1)**      | Constant time, independent of input size. |
| **O(log n)**  | Logarithmic time, reduces problem size by a factor each step. |
| **O(n)**      | Linear time, grows directly with input size. |
| **O(n log n)**| Linearithmic time, common in efficient sorting algorithms. |
| **O(n²)**     | Quadratic time, typically seen in nested loops. |
| **O(2ⁿ)**     | Exponential time, doubles with each step (e.g., recursion problems). |
| **O(n!)**     | Factorial time, seen in combinatorial problems (e.g., permutations). |

---

## Time Complexity Graphs

### 1. O(1) - Constant Time Complexity

```plaintext
| Time Complexity Graph for O(1)
|
|   |
|   |   * * * * * * * * * * * *
|   |__________________________
|            Input Size (n)
```

### 2. O(log n) - Logarithmic Time Complexity

```plaintext
| Time Complexity Graph for O(log n)
|
|   |
|   |          *
|   |         *
|   |        *
|   |      *
|   |   *
|   |__________________________
|            Input Size (n)
```

### 3. O(n) - Linear Time Complexity

```plaintext
| Time Complexity Graph for O(n)
|
|   |
|   |         *
|   |       *
|   |     *
|   |   *
|   | *
|   |__________________________
|            Input Size (n)
```

### 4. O(n log n) - Linearithmic Time Complexity

```plaintext
| Time Complexity Graph for O(n log n)
|
|   |
|   |             *
|   |          *
|   |       *
|   |     *
|   |  *
|   |__________________________
|            Input Size (n)
```

### 5. O(n²) - Quadratic Time Complexity

```plaintext
| Time Complexity Graph for O(n²)
|
|   |
|   |                  *
|   |              *
|   |           *
|   |        *
|   |     *
|   |__________________________
|            Input Size (n)
```

### 6. O(2ⁿ) - Exponential Time Complexity

```plaintext
| Time Complexity Graph for O(2ⁿ)
|
|   |
|   |                     *
|   |                 *
|   |            *
|   |        *
|   |     *
|   |__________________________
|            Input Size (n)
```

### 7. O(n!) - Factorial Time Complexity

```plaintext
| Time Complexity Graph for O(n!)
|
|   |
|   |                         *
|   |                    *
|   |                *
|   |          *
|   |      *
|   |__________________________
|            Input Size (n)
```

---

## Space Complexity

- **Definition**: Measures the memory used by an algorithm.
- **Common Complexities**: O(1), O(n), O(n²)



# Table of Contents

1. [Data Structures](#data-structures)
    - [Types of Data Structures](#types-of-data-structures)
    - [Linear Data Structures](#linear-data-structures)
    - [Non-Linear Data Structures](#non-linear-data-structures)
2. [Algorithms](#algorithms)
    - [Important Algorithms](#important-algorithms)
    - [Algorithm Categories](#algorithm-categories)
3. [Complexity Analysis](#complexity-analysis)
4. [Key Concepts](#key-concepts)

## Data Structures

Data structures are ways to organize and store data to perform operations efficiently.

### Types of Data Structures

1. **Linear Data Structures**: Data elements are arranged sequentially.
    - Examples: Arrays, Linked Lists, Stacks, Queues

2. **Non-Linear Data Structures**: Data elements are arranged hierarchically or in a complex relationship.
    - Examples: Trees, Graphs, Heaps


### Linear Data Structures

#### 1. Arrays
- **Description**: A collection of elements stored in contiguous memory locations.
- **Operations**: Search, Insert, Delete, Sort.
- **Use Case**: Efficient for storing a fixed number of items.

#### 2. Linked Lists
- **Description**: A sequence of nodes where each node contains data and a pointer to the next node.
- **Types**: Singly Linked List, Doubly Linked List, Circular Linked List.
- **Use Case**: Dynamic memory allocation, overcoming size constraints of arrays.

#### 3. Stacks
- **Description**: A LIFO (Last In, First Out) data structure.
- **Operations**: `push`, `pop`, `peek`.
- **Use Case**: Undo operations, expression evaluation.

#### 4. Queues
- **Description**: A FIFO (First In, First Out) data structure.
- **Types**: Simple Queue, Circular Queue, Priority Queue.
- **Use Case**: Task scheduling, buffering data.

#### 5. Hash Tables
- **Description**: Stores key-value pairs for fast lookup.
- **Operations**: Insert, Search, Delete.
- **Use Case**: Caching, lookup tables.


### Non-Linear Data Structures

#### 1. Trees
- **Description**: A hierarchical data structure with nodes connected by edges.
- **Types**: Binary Tree, Binary Search Tree (BST), AVL Tree, B-Tree, Trie.
- **Use Case**: Representing hierarchical data like file systems.

#### 2. Graphs
- **Description**: A collection of vertices (nodes) and edges.
- **Types**: Directed, Undirected, Weighted, Cyclic, Acyclic.
- **Use Case**: Social networks, routing algorithms.

#### 3. Heaps
- **Description**: A binary tree that maintains the heap property (min-heap or max-heap).
- **Use Case**: Priority queues, sorting algorithms (Heap Sort).

#### 4. Tries (Prefix Trees)
- **Description**: A tree for storing strings efficiently.
- **Use Case**: Auto-complete, dictionary lookups.


## Algorithms

Algorithms are step-by-step procedures to solve problems efficiently.

### Important Algorithms

1. **Searching Algorithms**
    - **Linear Search**: O(n)
    - **Binary Search**: O(log n)

2. **Sorting Algorithms**
    - **Bubble Sort**: O(n²)
    - **Insertion Sort**: O(n²)
    - **Merge Sort**: O(n log n)
    - **Quick Sort**: O(n log n) on average

3. **Graph Algorithms**
    - **Breadth-First Search (BFS)**: O(V + E)
    - **Depth-First Search (DFS)**: O(V + E)
    - **Dijkstra’s Algorithm**: Shortest path in weighted graphs.
    - **Kruskal’s Algorithm**: Minimum spanning tree.

4. **Dynamic Programming**
    - **Examples**: Fibonacci, Longest Common Subsequence (LCS), 0/1 Knapsack.

5. **Greedy Algorithms**
    - **Examples**: Huffman Coding, Prim’s Algorithm.


### Algorithm Categories

1. **Divide and Conquer**
    - Breaks a problem into smaller subproblems and solves them recursively.
    - **Examples**: Merge Sort, Quick Sort, Binary Search.

2. **Greedy Algorithms**
    - Makes the locally optimal choice at each step.
    - **Examples**: Kruskal’s Algorithm, Huffman Coding.

3. **Dynamic Programming (DP)**
    - Solves problems by combining solutions of overlapping subproblems.
    - **Examples**: Fibonacci, 0/1 Knapsack.

4. **Backtracking**
    - Explores all possible solutions.
    - **Examples**: N-Queens Problem, Sudoku Solver.


## Complexity Analysis

### Time Complexity
- **Definition**: Measures how the runtime of an algorithm grows with input size.
- **Common Complexities**: O(1), O(log n), O(n), O(n log n), O(n²)

### Space Complexity
- **Definition**: Measures the memory used by an algorithm.
- **Common Complexities**: O(1), O(n)

### Big O Notation Examples

- **O(1)**: Constant time.
- **O(n)**: Linear time.
- **O(n²)**: Quadratic time.
- **O(log n)**: Logarithmic time.


## Key Concepts

1. **Understanding Complexity**
    - Analyze the efficiency of your code using Big O notation.

2. **Problem-Solving Techniques**
    - Divide and Conquer, Greedy, Dynamic Programming, Backtracking.

3. **Practice Coding**
    - Platforms like **LeetCode**, **HackerRank**, and **Codeforces**.

4. **Mastering Data Structures**
    - Know when to use each structure based on constraints.

5. **Optimization**
    - Write clean, efficient code.


**By mastering these concepts, you can tackle a wide range of problems effectively!**
