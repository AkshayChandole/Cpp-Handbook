# [LRU Cache](#lru-cache)

## 🟢 LRU Cache

---

### 📌 Problem

Design a data structure that follows the constraints of a **Least Recently Used (LRU) cache**.

Implement the class:

* `LRUCache(int capacity)`
* `int get(int key)`
* `void put(int key, int value)`

### Rules:

* `get(key)` → return value if key exists, otherwise `-1`
* `put(key, value)` → insert/update the value
* When capacity exceeds → remove the **least recently used** item
* Both operations must run in **O(1)** time

---

### 📌 Example

```id="ex1"
Input:
["LRUCache","put","put","get","put","get","put","get","get","get"]
[[2],[1,1],[2,2],[1],[3,3],[2],[4,4],[1],[3],[4]]

Output:
[null,null,null,1,null,-1,null,-1,3,4]
```

---

# 🔴 Brute Force Solution

### 💡 Idea

Use:

* `vector<pair<int,int>>` to store cache
* On every `get`:

  * Search linearly
  * Move element to front
* On `put`:

  * If exists → update and move
  * Else insert
  * If full → remove last

---

### ❌ Problem

* Searching = O(N)
* Not acceptable (needs O(1))

---

### 💻 Code (Brute Force)

```cpp
class LRUCache {
    vector<pair<int,int>> cache;
    int capacity;

public:
    LRUCache(int capacity) {
        this->capacity = capacity;
    }

    int get(int key) {
        for(int i = 0; i < cache.size(); i++) {
            if(cache[i].first == key) {
                int value = cache[i].second;
                cache.erase(cache.begin() + i);
                cache.insert(cache.begin(), {key, value});
                return value;
            }
        }
        return -1;
    }

    void put(int key, int value) {
        for(int i = 0; i < cache.size(); i++) {
            if(cache[i].first == key) {
                cache.erase(cache.begin() + i);
                cache.insert(cache.begin(), {key, value});
                return;
            }
        }

        if(cache.size() == capacity) {
            cache.pop_back();
        }

        cache.insert(cache.begin(), {key, value});
    }
};
```

---

### ⏱ Time Complexity

O(N) per operation

### 🗂 Space Complexity

O(N)

---

# 🟡 Better Solution

### 💡 Idea

Use:

* `unordered_map<int, int>` → key → value
* `list<int>` → track order (most recent at front)

Problem:

* Removing arbitrary element from list requires iterator
* So we must store iterator in map

---

# 🟢 Optimum Solution (HashMap + Doubly Linked List)

### 💡 Core Idea

To achieve O(1):

1. **HashMap**

   * key → iterator of list

2. **Doubly Linked List**

   * Most recent → front
   * Least recent → back
   * Easy removal & insertion in O(1)

---

### 🔹 Data Structures

```cpp
list<pair<int,int>> dll;  // key, value
unordered_map<int, list<pair<int,int>>::iterator> mp;
```

---

### 💻 Code (Optimal)

```cpp
class LRUCache {
private:
    int capacity;
    list<pair<int,int>> dll; // {key, value}
    unordered_map<int, list<pair<int,int>>::iterator> mp;

public:
    LRUCache(int capacity) {
        this->capacity = capacity;
    }

    int get(int key) {
        if(mp.find(key) == mp.end())
            return -1;

        // Move accessed node to front
        auto node = mp[key];
        int value = node->second;
        dll.erase(node);
        dll.push_front({key, value});
        mp[key] = dll.begin();

        return value;
    }

    void put(int key, int value) {
        if(mp.find(key) != mp.end()) {
            dll.erase(mp[key]);
        }
        else if(dll.size() == capacity) {
            auto last = dll.back();
            mp.erase(last.first);
            dll.pop_back();
        }

        dll.push_front({key, value});
        mp[key] = dll.begin();
    }
};
```

---

### ⏱ Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| get       | O(1)       |
| put       | O(1)       |

---

### 🗂 Space Complexity

O(N)

---

# 🎯 Why Doubly Linked List?

Because we need:

* Remove arbitrary node in O(1)
* Insert at front in O(1)
* Remove last in O(1)

Singly linked list ❌
Vector ❌
Deque ❌

Only **DLL + HashMap** satisfies constraints.

---

# 🏆 Final Comparison

| Approach      | get  | put  | Suitable? |
| ------------- | ---- | ---- | --------- |
| Vector        | O(N) | O(N) | ❌         |
| Map + List    | O(1) | O(1) | ✅         |
| DLL + HashMap | O(1) | O(1) | ⭐ Optimal |

---

