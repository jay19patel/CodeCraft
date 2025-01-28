# Software Developer Preparation Guide

## **1. Data Structures and Algorithms (DSA)**

### **1.1 Foundational Topics**

#### **Linear Data Structures**

- **Arrays**:
  - Description: A collection of elements stored in contiguous memory locations.
  - Operations: Search, Insert, Delete, Sort.
  - Use Case: Efficient for storing a fixed number of items.

- **Strings**:
  - Common Techniques: Two-pointer methods, Sliding window.
  - Example Problem: Find the longest substring without repeating characters.

- **Linked Lists**:
  - Description: A sequence of nodes where each node contains data and a pointer to the next node.
  - Types: Singly Linked List, Doubly Linked List, Circular Linked List.
  - Example Problem: Reverse a linked list.

- **Stacks**:
  - Description: A LIFO (Last In, First Out) data structure.
  - Operations: `push`, `pop`, `peek`.
  - Use Case: Undo operations, expression evaluation.

- **Queues**:
  - Description: A FIFO (First In, First Out) data structure.
  - Types: Simple Queue, Circular Queue, Priority Queue.
  - Use Case: Task scheduling, buffering data.

- **Hash Tables**:
  - Description: Stores key-value pairs for fast lookup.
  - Operations: Insert, Search, Delete.
  - Use Case: Caching, lookup tables.

#### **Non-Linear Data Structures**

- **Trees**:
  - Description: A hierarchical data structure with nodes connected by edges.
  - Types: Binary Tree, Binary Search Tree (BST), AVL Tree, B-Tree, Trie.
  - Example Problem: Find the height of a binary tree.

- **Graphs**:
  - Description: A collection of vertices (nodes) and edges.
  - Types: Directed, Undirected, Weighted, Cyclic, Acyclic.
  - Applications: Social networks, routing algorithms.

- **Heaps**:
  - Description: A binary tree that maintains the heap property (min-heap or max-heap).
  - Use Case: Priority queues, sorting algorithms (Heap Sort).

- **Tries (Prefix Trees)**:
  - Description: A tree for storing strings efficiently.
  - Use Case: Auto-complete, dictionary lookups.

### **1.2 Algorithms**

#### **Important Categories**

- **Searching Algorithms**:
  - Linear Search: O(n)
  - Binary Search: O(log n)

- **Sorting Algorithms**:
  - Bubble Sort: O(n²)
  - Insertion Sort: O(n²)
  - Merge Sort: O(n log n)
  - Quick Sort: O(n log n) on average

- **Graph Algorithms**:
  - Breadth-First Search (BFS): O(V + E)
  - Depth-First Search (DFS): O(V + E)
  - Dijkstra’s Algorithm: Shortest path in weighted graphs.
  - Kruskal’s Algorithm: Minimum spanning tree.

- **Dynamic Programming (DP)**:
  - Examples: Fibonacci, Longest Common Subsequence (LCS), 0/1 Knapsack.

- **Greedy Algorithms**:
  - Examples: Huffman Coding, Prim’s Algorithm.

- **Divide and Conquer**:
  - Examples: Merge Sort, Quick Sort, Binary Search.

- **Backtracking**:
  - Examples: N-Queens Problem, Sudoku Solver.

#### **Advanced Algorithms**

- **Bloom Filter**:
  - Description: A probabilistic data structure to test membership with space efficiency.
  - Use Case: Cache filtering, detecting duplicate web pages.

- **Floyd-Warshall Algorithm**:
  - Description: All-pairs shortest path algorithm.

- **Tarjan’s Algorithm**:
  - Description: Strongly connected components detection in graphs.

- **KMP Algorithm**:
  - Description: Efficient string matching algorithm.

### **1.3 DSA Patterns**

- **Prefix Sum**
- **Two Pointers**
- **Sliding Window**
- **Fast and Slow Pointers**
- **Monotonic Stack**
- **Top K Elements**
- **Modified Binary Search**
- **Backtracking**
- **Dynamic Programming**
- **Matrix Traversal**
- **Tree Traversal**
- **Graph Traversal**

### **1.4 Complexity Analysis**

#### **Time Complexity**

| **Complexity** | **Description**                         |
|----------------|-----------------------------------------|
| **O(1)**      | Constant time, independent of input size. |
| **O(log n)**  | Logarithmic time, reduces problem size by a factor each step. |
| **O(n)**      | Linear time, grows directly with input size. |
| **O(n log n)**| Linearithmic time, common in efficient sorting algorithms. |
| **O(n²)**     | Quadratic time, typically seen in nested loops. |
| **O(2^n)**    | Exponential time, doubles with each step (e.g., recursion problems). |
| **O(n!)**     | Factorial time, seen in combinatorial problems (e.g., permutations). |

#### **Space Complexity**

| **Complexity** | **Description**                         |
|----------------|-----------------------------------------|
| **O(1)**      | Constant space, independent of input size. |
| **O(n)**      | Linear space, grows directly with input size. |

---

## **2. Object-Oriented Programming (OOPs)**

### **2.1 Core Concepts**

- **Encapsulation**: Bundling data and methods into a single unit (class).
- **Abstraction**: Hiding implementation details and showing only the essential features.
- **Inheritance**: Reusing and extending functionality of an existing class.
- **Polymorphism**: Same interface, different behaviors (method overloading/overriding).

### **2.2 Access Modifiers**

- **Private**: Accessible only within the class.
- **Protected**: Accessible within the class and its subclasses.
- **Public**: Accessible from anywhere.

Example:
```python
class Employee:
    def __init__(self, name, salary):
        self.name = name  # Public
        self.__salary = salary  # Private

    def display(self):
        print(f"Name: {self.name}, Salary: {self.__salary}")
```

### **2.3 Design Principles**

- **SOLID Principles**:
  - Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion.
- **DRY** (Don’t Repeat Yourself).
- **KISS** (Keep It Simple, Stupid).

### **2.4 Design Patterns**

- **Creational Patterns**: Singleton, Factory.
- **Structural Patterns**: Adapter, Decorator.
- **Behavioral Patterns**: Observer, Strategy.
