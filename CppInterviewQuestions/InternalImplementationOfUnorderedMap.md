## [How is std::unordered_map implemented?](#How-is-stdunordered_map-implemented)

### 🔹 std::unordered_map – Internal Implementation

`std::unordered_map` is implemented as a **hash table**.

It provides:

* ✅ Average O(1) insert
* ✅ Average O(1) find
* ✅ Average O(1) erase
* ❌ No ordering
* ❌ Worst case O(n) (hash collisions)

Let’s break it down properly.

---

## 1️⃣ Core Data Structure

Internally it contains:

```cpp id="a1xq9k"
template<typename Key, typename T>
struct Node {
    std::pair<const Key, T> data;
    Node* next;   // for collision chaining
};
```

And the main container:

```cpp id="c4mz7s"
std::vector<Node*> buckets;
size_t bucket_count;
size_t size_;
```

So conceptually:

```text id="m1u7qd"
buckets (array)
----------------------------------
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | ...
----------------------------------
  |        |
  v        v
 [k1]     [k2] -> [k3] -> [k4]
```

Each bucket stores a **linked list of nodes** (separate chaining).

---

## 2️⃣ How Hashing Works

On insert/find:

```cpp id="kp7b3n"
size_t index = hash(key) % bucket_count;
```

Default hash:

```cpp id="n6t9vf"
std::hash<Key>
```

Default comparator:

```cpp id="w3lqk1"
std::equal_to<Key>
```

---

## 3️⃣ Insertion Internals

#### Step 1 – Compute Bucket

```cpp id="i4osj2"
index = hash(key) % bucket_count;
```

#### Step 2 – Traverse Bucket Chain

```cpp id="txr8zd"
Node* curr = buckets[index];

while (curr) {
    if (curr->data.first == key)
        return;  // map does not allow duplicate keys
    curr = curr->next;
}
```

#### Step 3 – Insert at Head (common strategy)

```cpp id="y7kz3m"
Node* newNode = new Node{{key, value}, buckets[index]};
buckets[index] = newNode;
```

Time complexity: O(1) average.

---

## 4️⃣ Collision Resolution – Separate Chaining

If two keys hash to same bucket:

```text id="f3z9qe"
bucket[3]:
    [keyA] -> [keyB] -> [keyC]
```

This is linked list inside bucket.

Other strategies (not used in STL):

* Open addressing
* Quadratic probing
* Double hashing

STL typically uses chaining.

---

## 5️⃣ Load Factor & Rehashing

#### Load Factor

```cpp id="c2vy6k"
load_factor = size_ / bucket_count;
```

When:

```text id="p7l8xf"
load_factor > max_load_factor
```

→ Rehash triggered.

Default max_load_factor ≈ 1.0

---

## 6️⃣ Rehashing Internals

Steps:

1. Allocate new bucket array (usually 2x size)
2. Recompute bucket index for every element
3. Move nodes into new buckets

```cpp id="9wmls1"
for (each old bucket) {
    for (each node in chain) {
        size_t new_index = hash(node->key) % new_bucket_count;
        insert into new_buckets[new_index];
    }
}
```

Complexity: O(n)

But happens rarely → amortized O(1)

---

## 7️⃣ Why Average O(1)?

Because:

```text id="zq8dva"
If hash function distributes keys uniformly
```

Each bucket has few elements → constant traversal.

Worst case:

All keys in same bucket → O(n)

---

## 8️⃣ Iterator Implementation

Iterator must:

1. Traverse current bucket chain
2. Move to next non-empty bucket
3. Continue

Conceptually:

```cpp id="4h8n1z"
class iterator {
    Node* node;
    size_t bucket_index;
};
```

---

## 9️⃣ Memory Layout

Memory is not contiguous:

```text id="0v7lch"
buckets array (contiguous)
nodes allocated separately on heap
```

So:

* More memory overhead than vector
* Better than tree for lookup
* Less cache-friendly than vector

---

## 🔟 Erase Internals

To erase key:

1. Find bucket
2. Traverse chain
3. Fix linked list

```cpp id="3pqm8f"
prev->next = curr->next;
delete curr;
```

Average O(1)

---

## 1️⃣1️⃣ Why unordered_map Is Faster Than map?

Because:

| map            | unordered_map          |
| -------------- | ---------------------- |
| Tree traversal | Direct bucket indexing |
| Rotations      | None                   |
| O(log n)       | O(1) average           |
| Ordered        | Unordered              |

---

## 1️⃣2️⃣ Simplified Educational Implementation

```cpp id="n7gk4x"
#include <vector>
#include <functional>

template<typename Key, typename Value>
class SimpleUnorderedMap {
    struct Node {
        Key key;
        Value value;
        Node* next;
    };

    std::vector<Node*> buckets;
    size_t bucket_count;

public:
    SimpleUnorderedMap(size_t size = 8)
        : bucket_count(size), buckets(size, nullptr) {}

    void insert(const Key& key, const Value& value) {
        size_t index = std::hash<Key>{}(key) % bucket_count;

        Node* node = new Node{key, value, buckets[index]};
        buckets[index] = node;
    }
};
```

Real STL adds:

* Allocator support
* Exception safety
* Rehash policies
* Node handles (C++17)
* Transparent hashing (C++14+)
* Prime bucket sizing (libstdc++)
* Power-of-two bucket sizing (libc++)

---

## 1️⃣3️⃣ Advanced Implementation Details (Interview Gold)

Different libraries differ:

#### 🔹 libstdc++

* Uses prime bucket sizes
* Reduces clustering

#### 🔹 libc++

* Often uses power-of-two bucket counts
* Uses bitmask instead of modulo:

```cpp id="j0d0v1"
index = hash & (bucket_count - 1);
```

Faster than modulo.

---

## 1️⃣4️⃣ Iterator Invalidation Rules

| Operation | Invalidates         |
| --------- | ------------------- |
| insert    | Only if rehash      |
| erase     | Only erased element |
| rehash    | All iterators       |

---


## 🔥 Principal-Level Insight

Performance depends heavily on:

```text
Quality of hash function
Load factor
Bucket growth policy
```

In production systems, poor hashing destroys performance.

---

## 🚀 Summary

Internally:

```text
std::unordered_map
    → Hash function
    → Bucket array
    → Linked list chaining
    → Rehashing when load factor exceeded
```

Average complexity: O(1)
Worst case: O(n)

---

