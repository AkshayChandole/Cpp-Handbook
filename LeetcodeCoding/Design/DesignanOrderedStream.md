# [Design an Ordered Stream](#design-an-ordered-stream)

## 🟢 Design an Ordered Stream

---

### 📌 Problem

There is a stream of `(idKey, value)` pairs arriving in arbitrary order.

You must implement:

```cpp
OrderedStream(int n)
vector<string> insert(int idKey, string value)
```

Behavior:

* Store values at position `idKey`.
* Maintain a pointer `ptr` starting at 1.
* When inserting:

  * If inserted at `ptr`, return all consecutive values from `ptr` onward.
  * Otherwise return empty list.

---

### 📌 Example

```id="ex1"
Input:
["OrderedStream", "insert", "insert", "insert", "insert", "insert"]
[[5], [3,"ccccc"], [1,"aaaaa"], [2,"bbbbb"], [5,"eeeee"], [4,"ddddd"]]

Output:
[null, [], ["aaaaa"], ["bbbbb","ccccc"], [], ["ddddd","eeeee"]]
```

---

### 🔎 Explanation

After:

```id="step1"
insert(3,"ccccc") → []
```

Because pointer = 1.

Then:

```id="step2"
insert(1,"aaaaa") → ["aaaaa"]
```

Pointer moves to 2.

---

# 🔴 1. Brute Force (Map + Scan Every Time)

---

### 💡 Idea

Use `unordered_map<int,string>`.

On each insert:

* Scan from pointer forward.
* Collect consecutive elements.

---

### 💻 Code (C++)

```cpp id="bf1"
class OrderedStream {
public:
    unordered_map<int,string> mp;
    int ptr;

    OrderedStream(int n) {
        ptr = 1;
    }

    vector<string> insert(int idKey, string value) {
        mp[idKey] = value;
        vector<string> result;

        while(mp.count(ptr)) {
            result.push_back(mp[ptr]);
            ptr++;
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

Worst-case O(n²)

Repeated hash lookups.

❌ Not optimal.

---

# 🟡 2. Better Solution (Vector Instead of Map)

---

### 💡 Key Insight

Since:

```id="range"
1 ≤ idKey ≤ n
```

We can use:

```id="vector"
vector<string>
```

Direct indexing instead of map.

---

### 💻 Code (C++)

```cpp id="better1"
class OrderedStream {
public:
    vector<string> stream;
    int ptr;

    OrderedStream(int n) {
        stream.resize(n + 1);
        ptr = 1;
    }

    vector<string> insert(int idKey, string value) {
        stream[idKey] = value;
        vector<string> result;

        while(ptr < stream.size() &&
              !stream[ptr].empty()) {
            result.push_back(stream[ptr]);
            ptr++;
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

Amortized O(n) total
Each element visited once.

### 🗂 Space Complexity

O(n)

✅ Efficient.

---

# 🟢 3. Optimum Insight (Amortized Analysis)

---

### 💡 Why This Is Optimal

* Each index is processed exactly once.
* Pointer only moves forward.
* No backward scanning.

Total work across all inserts:

```id="amortized"
O(n)
```

So average per operation:

```id="avg"
O(1)
```

---

# 🔎 Dry Run

For:

```id="dry1"
n = 5
```

Initial:

```id="dry2"
stream = [_,_,_,_,_,_]
ptr = 1
```

Insert (3,"ccccc"):

```id="dry3"
stream[3] = "ccccc"
ptr = 1 → empty → return []
```

Insert (1,"aaaaa"):

```id="dry4"
stream[1] = "aaaaa"
collect index 1
ptr = 2
```

Return:

```id="dry5"
["aaaaa"]
```

---

# 🏆 Final Comparison

| Approach | Insert Time    | Space | Recommended |
| -------- | -------------- | ----- | ----------- |
| Map      | O(n) worst     | O(n)  | ❌           |
| Vector   | Amortized O(1) | O(n)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Array indexing
* Pointer maintenance
* Amortized complexity
* Data structure design

Related problems:

* Design HashMap
* Design Browser History
* Design Circular Queue
* LRU Cache

---

