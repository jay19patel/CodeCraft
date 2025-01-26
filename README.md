# Software Developer Preparation Notes

## **1. SQL (Structured Query Language)**

### **Basics**

- **DDL (Data Definition Language):**
  - CREATE, ALTER, DROP, TRUNCATE.
  - Example: `CREATE TABLE Employee (ID INT, Name VARCHAR(50));`
- **DML (Data Manipulation Language):**
  - INSERT, UPDATE, DELETE, SELECT.
  - Example: `INSERT INTO Employee VALUES (1, 'Jay');`
- **DCL (Data Control Language):**
  - GRANT, REVOKE.
  - Example: `GRANT SELECT ON Employee TO user_name;`
- **TCL (Transaction Control Language):**
  - COMMIT, ROLLBACK, SAVEPOINT.
  - Example: `SAVEPOINT save1;`

### **Intermediate**

- **Joins:**
  - INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL OUTER JOIN.
  - Example: `SELECT e.Name, d.DepartmentName FROM Employee e INNER JOIN Department d ON e.DeptID = d.ID;`
- **Subqueries and Correlated Subqueries**.
- **Aggregate Functions:**
  - COUNT, SUM, AVG, MIN, MAX.
- **Indexes:**
  - Clustered and Non-clustered indexes.
  - Example: `CREATE INDEX idx_name ON Employee (Name);`

### **Advanced**

- **Window Functions:**
  - RANK(), DENSE_RANK(), ROW_NUMBER(), PARTITION BY.
  - Example: `SELECT Name, RANK() OVER (PARTITION BY DeptID ORDER BY Salary DESC) FROM Employee;`
- **Query Optimization Techniques.**
- **Stored Procedures and Functions:**
  - Example: `CREATE PROCEDURE GetEmployeeDetails AS BEGIN SELECT * FROM Employee; END;`
- **Normalization & Denormalization.**
- **Database Sharding and Partitioning.**

---

## **2. Data Structures and Algorithms (DSA)**

### **Foundational Topics**

- **Arrays and Strings:**
  - Basics: Two-pointer techniques, Sliding window.
  - Example: Find the longest substring without repeating characters.
- **Linked Lists:**
  - Single, Doubly, and Circular linked lists.
  - Example: Reverse a linked list.
- **Stacks and Queues:**
  - Implementation using arrays and linked lists.
  - Applications: Parenthesis matching, Sliding window maximum.

### **Intermediate Topics**

- **Trees and Binary Search Trees (BST):**
  - Tree traversals (In-order, Pre-order, Post-order).
  - Example: Find the height of a binary tree.
- **Graphs:**
  - BFS, DFS, Shortest Path (Dijkstra, Bellman-Ford).
  - Applications: Cycle detection, Minimum Spanning Tree (Kruskal, Prim).
- **Hashing:**
  - Hash maps and sets, Collision handling.
  - Example: Two Sum problem.

### **Advanced Topics**

- **Dynamic Programming:**
  - Knapsack problem, Longest Increasing Subsequence.
- **Greedy Algorithms:**
  - Activity selection, Huffman encoding.
- **Divide and Conquer:**
  - Merge Sort, Quick Sort.
- **Advanced Graph Algorithms:**
  - Floyd-Warshall, Tarjan’s Algorithm (Strongly Connected Components).

---

## **3. Object-Oriented Programming (OOPs)**

### **Core Concepts**

- **Encapsulation, Abstraction, Inheritance, Polymorphism.**
- **Access Modifiers:**
  - Private, Public, Protected.
  - Example:
    ```python
    class Employee:
        def __init__(self, name, salary):
            self.name = name
            self.salary = salary
        def display(self):
            print(f"Name: {self.name}, Salary: {self.salary}")
    ```

### **Design Principles**

- SOLID principles (Single Responsibility, Open/Closed, etc.).
- DRY (Don’t Repeat Yourself), KISS (Keep It Simple, Stupid).

### **Design Patterns**

- Creational Patterns: Singleton, Factory.
- Structural Patterns: Adapter, Decorator.
- Behavioral Patterns: Observer, Strategy.

---

## **4. APIs (Application Programming Interfaces)**

### **Basics**

- **RESTful APIs:**
  - CRUD operations: GET, POST, PUT, DELETE.
  - Example: `GET /api/v1/employees`
- **Authentication and Authorization:**
  - OAuth 2.0, JWT.

### **Intermediate**

- **Rate Limiting and Throttling.**
- **API Documentation:**
  - Using Swagger or Postman.

### **Advanced**

- **GraphQL APIs:**
  - Querying nested data, Mutations.
  - Example:
    ```graphql
    query {
        employee(id: 1) {
            name
            department
        }
    }
    ```
- **Webhooks and Websockets.**

---

## **5. System Design (For Experienced Roles)**

### **Core Concepts**

- **Load Balancing, Caching, Database Replication.**
- **CAP Theorem:**
  - Consistency, Availability, Partition Tolerance.
- **Scalability:**
  - Vertical and Horizontal scaling.

### **Practical Design Scenarios**

- **Design a URL Shortener.**
- **Design a Scalable Chat Application.**
- **Design an E-commerce System.**

### **Tools and Technologies**

- Redis, RabbitMQ, Kafka.
- Microservices, Kubernetes, Docker.

---

## **6. Time Complexity**

### **Understanding Big-O Notation**

- Analyze time and space complexity of algorithms.
- Common complexities: O(1), O(log n), O(n), O(n log n), O(n^2).

### **Common Examples**

- Linear Search: O(n).
- Binary Search: O(log n).
- Sorting Algorithms:
  - Merge Sort: O(n log n).
  - Bubble Sort: O(n^2).

### **Optimization Techniques**

- Use efficient data structures.
- Avoid nested loops when possible.
- Memoization and Dynamic Programming.

---

## **7. Behavioral Questions (STAR Method)**

### **STAR Method Explained**

- **Situation**: Describe the context.
- **Task**: Explain the challenge.
- **Action**: Detail the steps taken.
- **Result**: Highlight the outcome.

### **Common Questions**

- “Tell me about a time you faced a technical challenge and how you resolved it.”
- “Describe a situation where you worked in a team to deliver a project.”
- “Give an example of a time you had to learn something new quickly.”

---

## **8. System Knowledge**

### **Databases**

- Normalization, Indexing, Transactions.
- SQL vs. NoSQL.

### **Networking**

- HTTP, HTTPS, TCP/IP, WebSockets, DNS.

### **Cloud Technologies**

- AWS, Google Cloud, Azure basics.
- Containerization (Docker), Orchestration (Kubernetes).

### **Operating Systems**

- Processes, Threads, Deadlocks, Scheduling.
- Virtual Memory, File Systems.

---

## **9. Key Soft Skills**

### **Communication**

- Clearly explain your thought process during problem-solving.
- Ask clarifying questions when needed.

### **Collaboration**

- Demonstrate teamwork and conflict resolution skills.

### **Confidence**

- Be honest about what you know and don’t know.

---

This document provides a roadmap to prepare comprehensively for FANG interviews. Dedicate time to practice each section, focusing on both theoretical and practical aspects.

