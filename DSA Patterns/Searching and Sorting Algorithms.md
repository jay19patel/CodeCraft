# 📘 Searching and Sorting Algorithms - DSA Notes

## 🧭 1. Searching Algorithms

Searching ka matlab hota hai kisi element ko data ke andar dhoondhna.

### 🔍 1.1 Linear Search

**Logic:** Har element ko ek-ek karke check karte hain.

```python
# Linear Search Example
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i  # mil gaya
    return -1  # nahi mila

arr = [10, 20, 30, 40, 50]
target = 30
print(linear_search(arr, target))  # Output: 2
```

**Use When:**

* List chhoti ho ya unsorted ho.

**Time Complexity:** O(n)

---

### 🔎 1.2 Binary Search

**Logic:** Sorted array ke beech se start karte hain aur half-half karte hain.

```python
# Binary Search Example (array should be sorted)
def binary_search(arr, target):
    low, high = 0, len(arr) - 1

    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1

    return -1

arr = [10, 20, 30, 40, 50]
target = 40
print(binary_search(arr, target))  # Output: 3
```

**Use When:**

* Array sorted ho.

**Time Complexity:** O(log n)

---

## 🔃 2. Sorting Algorithms

Sorting ka matlab hota hai elements ko kisi order mein lagana (increasing ya decreasing).

### 📌 2.1 Bubble Sort

**Logic:** Bar bar adjacent elements ko compare karke swap karte hain agar out of order hon.

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(0, n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]  # swap

arr = [64, 25, 12, 22, 11]
bubble_sort(arr)
print(arr)  # Output: [11, 12, 22, 25, 64]
```

**Time Complexity:** O(n^2)

**Use When:**

* Learning purpose ya very small dataset.

---

### ⚙️ 2.2 Selection Sort

**Logic:** Har position ke liye minimum element find kar ke usko correct position pe rakho.

```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_idx = i
        for j in range(i+1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]

arr = [64, 25, 12, 22, 11]
selection_sort(arr)
print(arr)  # Output: [11, 12, 22, 25, 64]
```

**Time Complexity:** O(n^2)

**Use When:**

* Understand basics of sorting.

---

### 🔄 2.3 Insertion Sort

**Logic:** Har element ko uski correct position mein insert karo left part mein.

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and key < arr[j]:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key

arr = [64, 25, 12, 22, 11]
insertion_sort(arr)
print(arr)  # Output: [11, 12, 22, 25, 64]
```

**Time Complexity:** O(n^2)

**Use When:**

* Data mostly sorted ho.

---

### ⚡ 2.4 Merge Sort

**Logic:** Divide and conquer – list ko do parts mein todho, sort karo, merge karo.

```python
def merge_sort(arr):
    if len(arr) > 1:
        mid = len(arr)//2
        L = arr[:mid]
        R = arr[mid:]

        merge_sort(L)
        merge_sort(R)

        i = j = k = 0
        while i < len(L) and j < len(R):
            if L[i] < R[j]:
                arr[k] = L[i]
                i += 1
            else:
                arr[k] = R[j]
                j += 1
            k += 1

        while i < len(L):
            arr[k] = L[i]
            i += 1
            k += 1

        while j < len(R):
            arr[k] = R[j]
            j += 1
            k += 1

arr = [38, 27, 43, 3, 9, 82, 10]
merge_sort(arr)
print(arr)  # Output: [3, 9, 10, 27, 38, 43, 82]
```

**Time Complexity:** O(n log n)

**Use When:**

* Large dataset, stable sorting chahiye.

---

### 🚀 2.5 Quick Sort

**Logic:** Ek pivot choose karo, usse chhote aur bade elements ko alag karo, recursively sort karo.

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    else:
        pivot = arr[0]
        less = [x for x in arr[1:] if x <= pivot]
        greater = [x for x in arr[1:] if x > pivot]
        return quick_sort(less) + [pivot] + quick_sort(greater)

arr = [10, 7, 8, 9, 1, 5]
arr = quick_sort(arr)
print(arr)  # Output: [1, 5, 7, 8, 9, 10]
```

**Time Complexity:**

* Average: O(n log n)
* Worst: O(n^2) (when array is already sorted)

**Use When:**

* Memory efficient sort chahiye (in-place).

---

## 🧠 Tips for Use:

| Task                       | Algorithm                           |
| -------------------------- | ----------------------------------- |
| Small unsorted list search | Linear Search                       |
| Sorted list search         | Binary Search                       |
| Small sorting task         | Bubble / Selection / Insertion Sort |
| Large sorting task         | Merge Sort / Quick Sort             |

---

