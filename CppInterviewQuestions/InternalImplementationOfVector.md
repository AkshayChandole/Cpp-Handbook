# [How is std::vector implemented internally?](#How-is-stdvector-implemented-internally)

## 1️⃣ Internal Data Structure

A typical vector internally maintains **three pointers**:

```cpp
template<typename T>
class Vector {
    T* start;          // beginning of allocated memory
    T* finish;         // one past last constructed element
    T* end_of_storage; // one past allocated memory block
};
```

Equivalent conceptual representation:

```
|<----------- allocated memory ----------->|
[start ........ finish .... end_of_storage]
         constructed         free space
```

#### Meaning:

| Pointer          | Meaning                   |
| ---------------- | ------------------------- |
| `start`          | Beginning of memory block |
| `finish`         | End of active elements    |
| `end_of_storage` | Total capacity boundary   |

---

## 2️⃣ Size & Capacity Calculation

```cpp
size()     = finish - start;
capacity() = end_of_storage - start;
```

O(1) operations.

---

## 3️⃣ Memory Allocation

Vector uses:

### 🔹 std::allocator

Internally:

```cpp
T* new_memory = allocator.allocate(new_capacity);
```

Then constructs elements using:

```cpp
allocator.construct(ptr, value);   // placement new
```

---

## 4️⃣ How push_back Works Internally

### Case 1: Capacity Available

```cpp
*finish = value;  // placement new
++finish;
```

O(1)

---

### Case 2: Capacity Full → Reallocation Happens

Steps:

1. Calculate new capacity (usually 2x growth)
2. Allocate new memory
3. Move/Copy existing elements
4. Destroy old elements
5. Deallocate old memory
6. Update pointers

Pseudo-implementation:

```cpp
void push_back(const T& value) {
    if (finish == end_of_storage) {
        size_t new_capacity = capacity() * 2;

        T* new_start = allocator.allocate(new_capacity);
        T* new_finish = new_start;

        // Move or copy elements
        for (T* p = start; p != finish; ++p) {
            allocator.construct(new_finish++, std::move(*p));
        }

        allocator.construct(new_finish++, value);

        // Destroy old
        for (T* p = start; p != finish; ++p) {
            allocator.destroy(p);
        }

        allocator.deallocate(start, capacity());

        start = new_start;
        finish = new_finish;
        end_of_storage = start + new_capacity;
    }
    else {
        allocator.construct(finish++, value);
    }
}
```

---

## 5️⃣ Growth Strategy

Most implementations use:

```
new_capacity = old_capacity * 1.5 or 2
```

Why?

* Prevent frequent reallocations
* Keep amortized push_back = O(1)

#### Amortized Complexity Explanation

Reallocations happen rarely → total cost spreads out.

---

## 6️⃣ Why Vector Is Contiguous

Memory layout:

```
| element0 | element1 | element2 | element3 |
```

Because it uses a **single memory block**, unlike list or deque.

That’s why:

✔ Random access O(1)
✔ Cache friendly
✔ Can use pointer arithmetic

---

## 7️⃣ Iterator Invalidation Rules (Important Interview Topic)

When does vector invalidate iterators?

| Operation                     | Invalidates?                |
| ----------------------------- | --------------------------- |
| push_back (no reallocation)   | No                          |
| push_back (with reallocation) | All                         |
| insert (middle)               | From insertion point onward |
| erase                         | From erased position onward |

Why?

Because memory shifts or entire block changes.

---

## 8️⃣ Exception Safety During Reallocation

Vector provides:

#### Strong Exception Guarantee

If move constructor throws:

* Old memory remains intact
* New memory cleaned up

Implementation logic:

1. Allocate new block
2. Move elements
3. If move fails → destroy new block only
4. Old block untouched

---

## 9️⃣ Move Semantics Optimization (C++11+)

During reallocation:

```cpp
std::move(*p)
```

Instead of copy.

If type has noexcept move constructor → used
Else → fallback to copy

This decision uses type traits internally.

---

## 🔟 Emplace Back Internals

```cpp
template<class... Args>
void emplace_back(Args&&... args) {
    allocator.construct(finish, std::forward<Args>(args)...);
    ++finish;
}
```

No temporary object created.

---

## 1️⃣1️⃣ Memory Destruction (Destructor)

```cpp
~vector() {
    for (T* p = start; p != finish; ++p)
        allocator.destroy(p);

    allocator.deallocate(start, capacity());
}
```

---

## 1️⃣2️⃣ Why vector Is So Fast

✔ Contiguous memory
✔ Amortized O(1) push
✔ Cache friendly
✔ Minimal per-element overhead
✔ Uses move semantics

---
