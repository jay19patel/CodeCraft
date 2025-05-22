# DSA Patterns Explained (For Beginners)

Yeh notes un logon ke liye hain jo DSA (Data Structures and Algorithms) seekhna shuru kar rahe hain. Har ek pattern ko hum easy language me samjhayenge, simple examples, real-life questions aur line-by-line code explanation ke saath. Plus: Kis type ke questions me kaunsa pattern use hota hai, woh bhi discuss kiya gaya hai.

---

## 1. Prefix Sum

### Concept:

Prefix Sum ka matlab hai kisi array ke elements ka running sum banana. Jab bar bar kisi subarray ka sum calculate karna ho, toh yeh method kaafi fast hoti hai.

### Kab Use Kare:

* Jab hume kisi range ka sum nikalna ho multiple times.
* Jaise: Subarray Sum, Even-Odd count in range, etc.

### DSA Problem Example:

**Q: Given an array, find the sum of elements between indices i and j (inclusive), multiple times.**

### Solution:

```python
def build_prefix_sum(arr):
    prefix_sum = [0] * (len(arr) + 1)
    for i in range(len(arr)):
        prefix_sum[i+1] = prefix_sum[i] + arr[i]
    return prefix_sum

arr = [1, 2, 3, 4, 5]
prefix = build_prefix_sum(arr)
i, j = 1, 3
print("Sum from index", i, "to", j, "=", prefix[j+1] - prefix[i])
```

### Explanation:

* Pehle cumulative sum banate hain.
* Jab bhi kisi range ka sum chahiye: `prefix[j+1] - prefix[i]` use karo.

---

## 2. Two Pointers

### Concept:

Do pointer use karte hain ek hi array par. Sorted array ke problems ke liye kaafi useful.

### Kab Use Kare:

* Jab array sorted ho.
* Jab pair, triplet ya subarray ka target match karna ho.

### DSA Problem Example:

**Q: Find two numbers in a sorted array whose sum is equal to a target.**

### Solution:

```python
arr = [1, 2, 3, 4, 6]
target = 6
left = 0
right = len(arr) - 1

while left < right:
    curr_sum = arr[left] + arr[right]
    if curr_sum == target:
        print("Pair found:", arr[left], arr[right])
        break
    elif curr_sum < target:
        left += 1
    else:
        right -= 1
```

### Explanation:

* Sorted array me do ends se start karo.
* Sum check karo aur accordingly pointers ko move karo.

---

## 3. Sliding Window

### Concept:

Fixed ya variable size window ko array me slide karte hain without re-computing everything.

### Kab Use Kare:

* Jab subarray ke sum, average, max/min nikalne ho.
* Fixed size window required ho.

### DSA Problem Example:

**Q: Find max sum of subarray of size k.**

### Solution:

```python
arr = [1, 4, 2, 10, 23, 3, 1, 0, 20]
k = 4
window_sum = sum(arr[:k])
max_sum = window_sum

for i in range(k, len(arr)):
    window_sum += arr[i] - arr[i - k]
    max_sum = max(max_sum, window_sum)
print("Max sum:", max_sum)
```

### Explanation:

* Pehle k elements ka sum liya.
* Fir window slide ki aur update kiya by removing 1st and adding next.

---

## 4. Fast and Slow Pointers

### Concept:

Ek pointer fast move karta hai, ek slow. Mostly cycle detection aur middle element ke liye use hota hai.

### Kab Use Kare:

* Linked list problems
* Detect loop/cycle
* Find middle element

### DSA Problem Example:

**Q: Find middle of linked list.**

### Solution:

```python
slow = head
fast = head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
print("Middle node is:", slow.val)
```

### Explanation:

* Fast pointer 2 step chalta hai, slow 1 step. Jab fast end pe pahuchta hai, slow middle pe hota hai.

---

## 5. Monotonic Stack

### Concept:

Stack jo increasing ya decreasing order maintain karta hai.

### Kab Use Kare:

* Next Greater Element
* Stock Span
* Temperature problems

### DSA Problem Example:

**Q: For each element, find the next greater element.**

### Solution:

```python
arr = [2, 1, 2, 4, 3]
stack = []
res = [-1] * len(arr)

for i in range(len(arr)):
    while stack and arr[i] > arr[stack[-1]]:
        idx = stack.pop()
        res[idx] = arr[i]
    stack.append(i)
print(res)
```

### Explanation:

* Stack me indices rakhte hain.
* Jab naya element bada milta hai to stack ka top uska next greater ho jata hai.

---

## 6. Top K Elements

### Concept:

Heap (priority queue) se top k elements fast nikal sakte hain.

### Kab Use Kare:

* Top k frequent
* Kth largest/smallest

### DSA Problem Example:

**Q: Find top 2 frequent elements.**

### Solution:

```python
from collections import Counter
import heapq

nums = [1,1,1,2,2,3]
k = 2
count = Counter(nums)
heap = heapq.nlargest(k, count.keys(), key=count.get)
print(heap)
```

---

## 7. Modified Binary Search

### Concept:

Normal binary search ko condition ke hisaab se tweak karte hain.

### Kab Use Kare:

* Rotated array
* First/Last occurrence

### DSA Problem Example:

**Q: Find first occurrence of target in sorted array.**

### Solution:

```python
def first_occurrence(arr, target):
    low, high = 0, len(arr)-1
    result = -1
    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == target:
            result = mid
            high = mid - 1  # keep looking on left
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
    return result
```

---

## 8. Backtracking

### Concept:

All possible options try karo, jo valid ho usme jao warna wapas aa jao (backtrack).

### Kab Use Kare:

* Subsets, permutations, combinations
* Sudoku, N-Queen

### DSA Problem Example:

**Q: Find all subsets of array.**

### Solution:

```python
def backtrack(start, path):
    res.append(path[:])
    for i in range(start, len(nums)):
        path.append(nums[i])
        backtrack(i+1, path)
        path.pop()

nums = [1,2,3]
res = []
backtrack(0, [])
print(res)
```

---

## 9. Dynamic Programming (DP)

### Concept:

Big problem ko chhoti problems me tod ke store karo result (memoization/tabulation).

### Kab Use Kare:

* Fibonacci, Coin Change, Knapsack
* Optimal solutions from overlapping subproblems

### DSA Problem Example:

**Q: Find nth Fibonacci number.**

### Solution:

```python
dp = [0] * (n+1)
dp[1] = 1
for i in range(2, n+1):
    dp[i] = dp[i-1] + dp[i-2]
print(dp[n])
```

---

## 10. Matrix Traversal

### Concept:

Matrix me element access karne ke liye nested loop ya DFS/BFS ka use.

### Kab Use Kare:

* Grid based problems
* Islands, Maze, Pathfinding

### DSA Problem Example:

**Q: Count number of islands (1s connected in matrix).**

### Solution:

```python
def dfs(i, j):
    if i<0 or j<0 or i>=m or j>=n or grid[i][j]=='0':
        return
    grid[i][j] = '0'
    dfs(i+1,j); dfs(i-1,j); dfs(i,j+1); dfs(i,j-1)

count = 0
for i in range(m):
    for j in range(n):
        if grid[i][j]=='1':
            dfs(i,j)
            count += 1
```

---

## 11. Tree Traversal

### Concept:

* DFS: Inorder, Preorder, Postorder (Recursive/Stack)
* BFS: Level order (Queue)

### Kab Use Kare:

* Binary Trees, N-ary Trees

### DSA Problem Example:

**Q: Inorder traversal of binary tree.**

### Solution:

```python
def inorder(root):
    if root:
        inorder(root.left)
        print(root.val)
        inorder(root.right)
```

---

## 12. Graph Traversal

### Concept:

* DFS (stack/recursion)
* BFS (queue)

### Kab Use Kare:

* Graph search, Connected components, Shortest path (BFS)

### DSA Problem Example:

**Q: Number of connected components in undirected graph.**

### Solution:

```python
def dfs(node):
    visited.add(node)
    for nei in graph[node]:
        if nei not in visited:
            dfs(nei)

visited = set()
components = 0
for node in graph:
    if node not in visited:
        dfs(node)
        components += 1
```

---

## 13: Greedy

### Concept:

Har step pe best decision lena, bina future soch ke.

### Kab Use Kare:

* Job scheduling, Activity selection, Minimum coins

### DSA Problem Example:

**Q: Minimum coins to make change.**

### Solution:

```python
coins = [1, 2, 5]
amount = 11
coins.sort(reverse=True)
count = 0
for coin in coins:
    while amount >= coin:
        amount -= coin
        count += 1
print(count)
```

---

Next steps: Har pattern ke upar based real-life DSA problems ka set solve karte hain step-by-step. 🔥

Happy Learning! 🚀
