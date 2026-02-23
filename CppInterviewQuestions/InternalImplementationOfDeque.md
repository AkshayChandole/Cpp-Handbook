# [How is std::deque implemented internally?](#How-is-stddeque-implemented-internally)

 ### 🔹 std::deque – Internal Implementation

`std::deque` (Double Ended Queue) is **NOT contiguous like vector**.

Instead, it is implemented as:

> ✅ A **map (array of pointers)**
> ✅ Each pointer points to a **fixed-size memory block (buffer)**
> ✅ Allows fast insertion/removal at **both ends**

---

## 1️⃣ High-Level Structure

Internally, deque looks like this:

```
            map (array of pointers)
         ┌──────┬──────┬──────┬──────┬──────┐
         │  *   │  *   │  *   │  *   │  *   │
         └───┬──┴───┬──┴───┬──┴───┬──┴───┬──┘
             ↓      ↓      ↓      ↓
        [block0] [block1] [block2] [block3]
```

Each block is a small contiguous array.

---

## 2️⃣ Why This Design?

If deque used one contiguous block like vector:

* Inserting at front would require shifting all elements ❌

Instead:

* It just allocates a new block at front
* Updates pointers
* O(1) insertion at both ends ✅

---

## 3️⃣ Internal Data Members (Conceptual)

```cpp
template<typename T>
class Deque {
    T** map;               // pointer to array of block pointers
    size_t map_size;       // size of map

    T* start_cur;          // current element pointer
    T* start_first;        // first element of first block
    T* start_last;         // last position of first block

    T* finish_cur;         // last element pointer
    T* finish_first;
    T* finish_last;
};
```

Real implementations vary (libstdc++, libc++), but concept is same.

---

## 4️⃣ Block Size

Block size is usually:

```
block_size = max(1, 4096 / sizeof(T))
```

Why?

* Keep each block around 4KB (cache friendly)
* Avoid very large allocations

Example:

If `sizeof(int) = 4`

```
4096 / 4 = 1024 elements per block
```

---

## 5️⃣ Random Access (Important)

Even though memory is not contiguous,
deque still provides:

```cpp
O(1) operator[]
```

How?

Index calculation:

```cpp
size_t block_index = index / block_size;
size_t offset = index % block_size;

return map[block_index][offset];
```

So access is:

* One division
* One modulo
* Two pointer dereferences

Slightly slower than vector but still O(1).

---

## 6️⃣ push_back() Internals

#### Case 1: Space in Current Block

```cpp
*finish_cur = value;
++finish_cur;
```

---

#### Case 2: Block Full

1. Allocate new block
2. Add pointer in map
3. Move finish to new block
4. Insert element

If map array is full:

* Allocate larger map
* Copy old block pointers
* Update center position

---

## 7️⃣ push_front() Internals

Symmetric to push_back:

If space before start_cur → just decrement pointer.

If block full:

* Allocate new block at front
* Update map

No shifting of elements required.

---

## 8️⃣ Why Deque Is Not Contiguous

Memory layout:

```
[block1]   [block2]   [block3]
  0 1 2      3 4 5      6 7 8
```

Blocks may not be adjacent in memory.

Therefore:

❌ Cannot pass deque data to C APIs expecting contiguous memory
❌ data() is not guaranteed contiguous (until C++23 improvements)

---

## 9️⃣ Iterator Implementation

Deque iterator internally stores:

```cpp
struct iterator {
    T* cur;        // current element
    T* first;      // first element of block
    T* last;       // end of block
    T** node;      // pointer to block pointer in map
};
```

When incrementing:

```cpp
++cur;
if (cur == last) {
    node++;
    first = *node;
    last = first + block_size;
    cur = first;
}
```

So iterator knows:

* Current element
* Current block
* Map position

That’s why deque iterator is more complex than vector iterator.

---

## 🔟 Complexity Summary

| Operation       | Complexity     |
| --------------- | -------------- |
| Random access   | O(1)           |
| push_back       | O(1) amortized |
| push_front      | O(1) amortized |
| Insert middle   | O(n)           |
| Memory locality | Medium         |

---

## 1️⃣1️⃣ Iterator Invalidation Rules

| Operation     | Invalidates             |
| ------------- | ----------------------- |
| push_back     | Only if map reallocated |
| push_front    | Same                    |
| insert middle | Many iterators          |

Deque has **better iterator stability than vector**, but worse than list.

---

## 1️⃣2️⃣ Comparison: Vector vs Deque

| Feature         | vector         | deque          |
| --------------- | -------------- | -------------- |
| Contiguous      | Yes            | No             |
| Push front      | O(n)           | O(1)           |
| Push back       | O(1) amortized | O(1) amortized |
| Cache friendly  | Very high      | Medium         |
| Memory overhead | Low            | Higher         |

---
