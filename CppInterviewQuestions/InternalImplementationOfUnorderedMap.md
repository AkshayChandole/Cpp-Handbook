# [How is std::unordered_map implemented?](#How-is-stdunordered_map-implemented)

### 🔹 std::multimap vs std::map

#### Internal Differences

At implementation level, **both are typically built on the same Red-Black Tree structure**.

The real difference is **duplicate key handling**.

---

## 1️⃣ Core Data Structure

Both use:

> ✅ Red-Black Tree
> ✅ Node-based container
> ✅ Ordered by comparator (`std::less<Key>` by default)
> ✅ O(log n) insert / erase / find

Node structure is essentially identical:

```cpp
template<typename Key, typename T>
struct Node {
    std::pair<const Key, T> data;
    Node* parent;
    Node* left;
    Node* right;
    bool isRed;
};
```

So structurally:

```
map         → unique keys
multimap    → duplicate keys allowed
```

---

## 2️⃣ Major Internal Difference – Insertion Logic

### 🔹 std::map

During insertion:

```cpp
if (key < node->key)
    go left;
else if (key > node->key)
    go right;
else
    key already exists → reject insert
```

Only one node per key.

---

### 🔹 std::multimap

Insertion logic changes:

```cpp
if (key < node->key)
    go left;
else
    go right;   // even if equal
```

⚠ Equal keys are allowed.

This means:

* Duplicates are inserted
* Typically inserted in the **right subtree of equal key**
* Maintains sorted grouping of equal keys

---

## 3️⃣ Tree Shape with Duplicates

Example inserting:

```
(5,A)
(5,B)
(5,C)
```

Tree might look like:

```
        5(A)
           \
            5(B)
               \
                5(C)
```

Red-black balancing will still apply, so final structure is balanced.

Important:

All equal keys appear consecutively in inorder traversal.

---

## 4️⃣ Lookup Behavior Difference

### 🔹 map::find(key)

Returns:

```
Single iterator
```

---

### 🔹 multimap::find(key)

Returns:

```
Iterator to first occurrence
```

But duplicates may exist.

To access all duplicates:

```cpp
auto range = mm.equal_range(key);

for (auto it = range.first; it != range.second; ++it) {
    // process all duplicates
}
```

Internally:

* `lower_bound(key)` → first equal
* `upper_bound(key)` → first greater

Range between them are duplicates.

---

## 5️⃣ equal_range Internal Logic

Implemented via two tree searches:

```cpp
lower_bound → first node where !(node->key < key)
upper_bound → first node where (key < node->key)
```

Complexity: O(log n)

---

## 6️⃣ Why Complexity Stays O(log n)

Even with duplicates:

* Red-black properties still maintained
* Height still bounded by 2*log(n)

Worst case many duplicates still balanced.

---

## 7️⃣ Iterator Stability

Same as map:

| Operation | Invalidates      |
| --------- | ---------------- |
| insert    | No               |
| erase     | Only erased node |
| clear     | All              |

Because node-based tree.

---

## 8️⃣ Memory Overhead

Same as map:

* 3 pointers
* color bit
* pair<const Key, T>

Only difference:

multimap may store multiple nodes with same key → more memory.

---

## 9️⃣ Behavioral Difference at API Level

| Feature             | map           | multimap       |
| ------------------- | ------------- | -------------- |
| Duplicate keys      | ❌ No          | ✅ Yes          |
| operator[]          | ✅ Yes         | ❌ No           |
| insert return       | pair<it,bool> | iterator only  |
| equal_range useful? | Rare          | Very important |

---

## 🔟 Why multimap Has No operator[]?

Because:

```cpp
mm[key] = value;
```

What should happen?

* Overwrite one?
* Insert new duplicate?
* Which one?

Ambiguous → not provided.

---

## 1️⃣1️⃣ Under the Hood – Template Reuse

In libstdc++ and libc++:

Both map and multimap reuse the same internal tree class.

Conceptually:

```cpp
template<bool IsMulti>
class Tree;
```

* `IsMulti = false` → map
* `IsMulti = true` → multimap

Difference handled during insert logic.

---



## 🔥 Principal-Level Insight

Even though duplicates may cluster:

Red-Black balancing ensures:

```
Tree height remains logarithmic
```

Duplicates don’t turn it into linked list.

---

## 🚀 Summary

Internally:

```
std::map        → Red-Black Tree + unique key constraint
std::multimap   → Same Red-Black Tree + allow duplicates
```

Difference is purely in:

* Insertion rule
* Lookup behavior
* API restrictions

---
