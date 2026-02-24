# [How is std::unordered_set implemented?](#How-is-stdunordered_set-implemented)

### 🔹 std::unordered_set – Internal Implementation

`std::unordered_set` is implemented as a **hash table using separate chaining** (in most STL implementations).

It is almost identical to `std::unordered_map`, except:

> 🔹 `unordered_map` stores `key → value`
> 🔹 `unordered_set` stores **only keys**

---

## 1️⃣ Core Data Structure

Internally:

```cpp
template<typename Key>
struct Node {
    Key key;
    Node* next;   // linked list for collisions
};
```

Container holds:

```cpp
std::vector<Node*> buckets;   // bucket array
size_t bucket_count;
size_t size_;
float max_load_factor_;
```

Conceptually:

```text
Bucket Array
--------------------------------
| 0 | 1 | 2 | 3 | 4 | 5 | 6 |
--------------------------------
      |         |
      v         v
     [A]      [B] -> [C]
```

Each bucket contains a linked list of nodes.

---

## 2️⃣ Hashing Process

When inserting or searching:

```cpp
size_t index = std::hash<Key>{}(key) % bucket_count;
```

Default components:

* Hash: `std::hash<Key>`
* Comparator: `std::equal_to<Key>`

---

## 3️⃣ Insertion Internals

#### Step 1 – Compute bucket

```cpp
index = hash(key) % bucket_count;
```

#### Step 2 – Check for duplicate

```cpp
Node* curr = buckets[index];

while (curr) {
    if (curr->key == key)
        return;   // set does NOT allow duplicates
    curr = curr->next;
}
```

#### Step 3 – Insert at head (common strategy)

```cpp
Node* newNode = new Node{key, buckets[index]};
buckets[index] = newNode;
size_++;
```

Average complexity: O(1)

---

## 4️⃣ Collision Resolution – Separate Chaining

If multiple keys hash to same bucket:

```text
bucket[2]:
    [10] -> [42] -> [99]
```

This is called **separate chaining**.

Other collision strategies (not used by STL):

* Open addressing
* Linear probing
* Quadratic probing

STL typically uses chaining because:

* Easier deletion
* Stable node addresses
* Simpler rehashing

---

## 5️⃣ Lookup Internals

```cpp
bool contains(const Key& key) {
    size_t index = hash(key) % bucket_count;

    Node* curr = buckets[index];
    while (curr) {
        if (curr->key == key)
            return true;
        curr = curr->next;
    }
    return false;
}
```

Average: O(1)
Worst case: O(n) (if all keys collide)

---

## 6️⃣ Load Factor & Rehashing

Load factor:

```cpp
load_factor = size_ / bucket_count;
```

If:

```text
load_factor > max_load_factor
```

→ Rehash triggered.

Default `max_load_factor ≈ 1.0`

---

## 7️⃣ Rehashing Internals

Steps:

1. Allocate new bucket array (often 2x size)
2. Recompute bucket index for every key
3. Move nodes

```cpp
for each old bucket:
    for each node:
        new_index = hash(node->key) % new_bucket_count
        move node
```

Time complexity: O(n)

But happens rarely → amortized O(1).

---

## 8️⃣ Memory Layout

Memory structure:

```text
[ Bucket Array (contiguous) ]
[ Node heap allocations ]
[ Node heap allocations ]
```

Characteristics:

* Buckets contiguous
* Nodes separately allocated
* More memory overhead than vector
* Better lookup than tree

---

## 9️⃣ Iterator Implementation

Iterator must:

1. Walk current linked list
2. Jump to next non-empty bucket

Conceptually:

```cpp
class iterator {
    Node* node;
    size_t bucket_index;
};
```

Traversal is NOT ordered.

---

## 🔟 Iterator Invalidation Rules

| Operation | Invalidates         |
| --------- | ------------------- |
| insert    | Only if rehash      |
| erase     | Only erased element |
| rehash    | All iterators       |

Because rehash moves nodes across buckets.

---

## 1️⃣1️⃣ Why unordered_set Is Faster Than set?

| set            | unordered_set |
| -------------- | ------------- |
| Red-Black Tree | Hash table    |
| O(log n)       | O(1) average  |
| Ordered        | Unordered     |
| Tree rotations | None          |

But worst case performance can degrade.

---

## 1️⃣2️⃣ Simplified Educational Implementation

```cpp
#include <vector>
#include <functional>

template<typename Key>
class SimpleUnorderedSet {
    struct Node {
        Key key;
        Node* next;
    };

    std::vector<Node*> buckets;
    size_t bucket_count;

public:
    SimpleUnorderedSet(size_t size = 8)
        : bucket_count(size), buckets(size, nullptr) {}

    void insert(const Key& key) {
        size_t index = std::hash<Key>{}(key) % bucket_count;

        Node* curr = buckets[index];
        while (curr) {
            if (curr->key == key)
                return;  // no duplicates
            curr = curr->next;
        }

        buckets[index] = new Node{key, buckets[index]};
    }
};
```

Real STL adds:

* Allocator support
* Exception safety
* Rehash policy
* Transparent hashing (C++14+)
* Node extraction (C++17)
* Optimized bucket sizing (prime or power-of-two)

---

## 1️⃣3️⃣ Implementation Differences Across Libraries

#### 🔹 libstdc++

* Often uses **prime bucket counts**
* Reduces clustering

#### 🔹 libc++

* Often uses **power-of-two bucket sizes**
* Uses bitmask instead of modulo:

```cpp
index = hash & (bucket_count - 1);
```

Faster than `%`.

---



## 🚀 Summary

Internally:

```text
std::unordered_set
    → Hash function
    → Bucket array
    → Linked list per bucket
    → Rehash when load factor exceeded
```

Average complexity: O(1)
Worst case: O(n)

---

