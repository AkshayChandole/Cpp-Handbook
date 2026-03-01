# [Insert Delete GetRandom](#insert-delete-getrandom)


## 🟢 Insert Delete GetRandom O(1)

---

### 📌 Problem

Design a data structure that supports:

* `insert(val)` → Inserts an item if not present. Returns `true` if inserted.
* `remove(val)` → Removes an item if present. Returns `true` if removed.
* `getRandom()` → Returns a random element from current elements.

⚠ All operations must run in **average O(1) time**.

---

### 📌 Example

```id="ex1"
Input:
["RandomizedSet","insert","remove","insert","getRandom","remove","insert","getRandom"]
[[],[1],[2],[2],[],[1],[2],[]]

Output:
[null,true,false,true,2,true,false,2]
```

---

# 🔴 1. Brute Force (Using Vector Only)

### 💡 Idea

* Store elements in a vector.
* `insert` → check existence (linear search).
* `remove` → find and erase.
* `getRandom` → pick random index.

---

### 💻 Code (C++)

```cpp id="bf1"
class RandomizedSet {
private:
    vector<int> nums;

public:
    bool insert(int val) {
        if(find(nums.begin(), nums.end(), val) != nums.end())
            return false;

        nums.push_back(val);
        return true;
    }

    bool remove(int val) {
        auto it = find(nums.begin(), nums.end(), val);
        if(it == nums.end())
            return false;

        nums.erase(it);
        return true;
    }

    int getRandom() {
        return nums[rand() % nums.size()];
    }
};
```

---

### ⏱ Time Complexity

* insert → O(N)
* remove → O(N)
* getRandom → O(1)

❌ Not acceptable.

---

# 🟡 2. Better (HashMap + Vector but Normal Erase)

### 💡 Idea

* Use `unordered_map` to check existence in O(1)
* Use vector for random access
* But still use `erase()` → O(N)

Still not fully O(1).

---

# 🟢 3. Optimum Solution (HashMap + Vector + Swap Trick)

### 💡 Core Idea

We need:

1. O(1) insert
2. O(1) delete
3. O(1) random access

Use:

* `vector<int> nums` → store elements
* `unordered_map<int, int> mp`

  * value → index in vector

---

### 🔥 Key Trick for O(1) Deletion

When removing an element:

1. Get its index from map.
2. Replace it with last element in vector.
3. Update index of last element in map.
4. Pop back vector.
5. Remove from map.

This avoids shifting elements.

---

### 💻 Code (C++)

```cpp id="opt1"
class RandomizedSet {
private:
    vector<int> nums;
    unordered_map<int, int> mp; // value -> index

public:
    RandomizedSet() {}

    bool insert(int val) {
        if(mp.count(val))
            return false;

        nums.push_back(val);
        mp[val] = nums.size() - 1;
        return true;
    }

    bool remove(int val) {
        if(!mp.count(val))
            return false;

        int index = mp[val];
        int lastElement = nums.back();

        // Move last element to index of removed element
        nums[index] = lastElement;
        mp[lastElement] = index;

        // Remove last element
        nums.pop_back();
        mp.erase(val);

        return true;
    }

    int getRandom() {
        return nums[rand() % nums.size()];
    }
};
```

---

### ⏱ Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| insert    | O(1)       |
| remove    | O(1)       |
| getRandom | O(1)       |

---

### 🗂 Space Complexity

O(N)

---

# 🎯 Why This Works

* Vector gives O(1) random access
* HashMap gives O(1) lookup
* Swap trick avoids O(N) erase shift

This pattern is very common in system design interviews.

---

# 🏆 Final Comparison

| Approach             | insert | remove | getRandom | Suitable? |
| -------------------- | ------ | ------ | --------- | --------- |
| Vector Only          | O(N)   | O(N)   | O(1)      | ❌         |
| Map + Vector (erase) | O(1)   | O(N)   | O(1)      | ❌         |
| Map + Vector + Swap  | O(1)   | O(1)   | O(1)      | ⭐ Best    |

---
