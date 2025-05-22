# 🧠 Advanced Algorithms - DSA Notes

Yeh notes basic se samjha rahe hain kuch important advanced algorithms jo interviews aur real-world applications mein kaam aate hain.

---

## 🌸 1. Bloom Filter

### 📌 What is it?

Bloom filter ek **probabilistic data structure** hai jo batata hai ki koi element set mein ho sakta hai ya nahi.

* **False Positive**: Ho sakta hai "element hai" bol de jabki nahi ho.
* **False Negative**: Kabhi nahi hota.

### 📦 Use Cases

* Large data sets mein fast lookup (ex: spell checker, cache system, network routers)

### 🧠 Idea

* Bit array + multiple hash functions

```python
import mmh3
from bitarray import bitarray

class BloomFilter:
    def __init__(self, size, hash_count):
        self.size = size
        self.hash_count = hash_count
        self.bit_array = bitarray(size)
        self.bit_array.setall(0)

    def add(self, item):
        for i in range(self.hash_count):
            index = mmh3.hash(item, i) % self.size
            self.bit_array[index] = 1

    def check(self, item):
        for i in range(self.hash_count):
            index = mmh3.hash(item, i) % self.size
            if self.bit_array[index] == 0:
                return False
        return True

bloom = BloomFilter(1000, 4)
bloom.add("apple")
print(bloom.check("apple"))  # True
print(bloom.check("banana")) # False (maybe True)
```

---

## 🏁 2. Floyd-Warshall Algorithm

### 📌 What is it?

Graph mein har node se har node ka **shortest distance** nikalne ka algorithm.

* Dynamic Programming pe based hai.
* Works with **negative weights** (but no negative cycles).

### 📦 Use Case

* All-Pairs Shortest Path (like Google Maps matrix distance)

```python
def floyd_warshall(graph):
    V = len(graph)
    dist = list(map(lambda i: list(i), graph))

    for k in range(V):
        for i in range(V):
            for j in range(V):
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    return dist

INF = float('inf')
graph = [
    [0, 5, INF, 10],
    [INF, 0, 3, INF],
    [INF, INF, 0, 1],
    [INF, INF, INF, 0]
]

result = floyd_warshall(graph)
for row in result:
    print(row)
```

---

## 🌊 3. Tarjan’s Algorithm

### 📌 What is it?

**Strongly Connected Components (SCC)** nikalta hai directed graph mein.

* DFS-based hai
* Low-link value use karta hai

### 📦 Use Case

* Circuit Analysis
* Finding modules in systems

```python
from collections import defaultdict

class TarjanSCC:
    def __init__(self, n):
        self.graph = defaultdict(list)
        self.index = 0
        self.stack = []
        self.index_map = [-1]*n
        self.lowlink = [0]*n
        self.on_stack = [False]*n
        self.sccs = []

    def add_edge(self, u, v):
        self.graph[u].append(v)

    def strongconnect(self, v):
        self.index_map[v] = self.lowlink[v] = self.index
        self.index += 1
        self.stack.append(v)
        self.on_stack[v] = True

        for w in self.graph[v]:
            if self.index_map[w] == -1:
                self.strongconnect(w)
                self.lowlink[v] = min(self.lowlink[v], self.lowlink[w])
            elif self.on_stack[w]:
                self.lowlink[v] = min(self.lowlink[v], self.index_map[w])

        if self.lowlink[v] == self.index_map[v]:
            scc = []
            while True:
                w = self.stack.pop()
                self.on_stack[w] = False
                scc.append(w)
                if w == v:
                    break
            self.sccs.append(scc)

    def get_sccs(self):
        for v in range(len(self.index_map)):
            if self.index_map[v] == -1:
                self.strongconnect(v)
        return self.sccs

# Example:
graph = TarjanSCC(5)
graph.add_edge(1, 0)
graph.add_edge(0, 2)
graph.add_edge(2, 1)
graph.add_edge(0, 3)
graph.add_edge(3, 4)
print(graph.get_sccs())
```

---

## 🔠 4. KMP Algorithm (Knuth-Morris-Pratt)

### 📌 What is it?

String pattern matching algorithm hai jo **efficient** hai.

* Naive method O(m\*n) time leta hai
* KMP O(m + n) mein karta hai

### 📦 Use Case

* Searching substrings (ex: spell check, word highlight, autocomplete)

### ⚙️ Logic:

* "Prefix table" banate hain jisme patter ke liye proper shift calculate hota hai

```python
def compute_lps(pattern):
    lps = [0] * len(pattern)
    length = 0
    i = 1
    while i < len(pattern):
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                length = lps[length-1]
            else:
                lps[i] = 0
                i += 1
    return lps

def kmp_search(text, pattern):
    lps = compute_lps(pattern)
    i = j = 0
    while i < len(text):
        if text[i] == pattern[j]:
            i += 1
            j += 1
        if j == len(pattern):
            print("Pattern found at index", i-j)
            j = lps[j-1]
        elif i < len(text) and text[i] != pattern[j]:
            if j != 0:
                j = lps[j-1]
            else:
                i += 1

kmp_search("abxabcabcaby", "abcaby")
```

---

