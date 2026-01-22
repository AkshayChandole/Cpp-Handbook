

# [STL CheatSheet](#stl-cheatsheet)
---

# 📦 1. Sequence Containers

---

## 🔹 `std::vector<T>` — Dynamic array (contiguous memory)

```cpp
v.push_back(x);          // Add element at end (may reallocate)
v.emplace_back(args);    // Construct element in place at end
v.pop_back();            // Remove last element

v.size();                // Number of elements
v.capacity();            // Allocated storage capacity
v.reserve(n);            // Reserve capacity (avoid reallocation)
v.resize(n);             // Resize vector (construct/remove elements)

v.clear();               // Remove all elements
v.empty();               // Check if vector is empty

v.front();               // First element
v.back();                // Last element
v[i];                    // Access element (no bounds check)
v.at(i);                 // Access element (throws if out of range)

v.begin();               // Iterator to first element
v.end();                 // Iterator past last element
v.rbegin();              // Reverse iterator to last element
v.rend();                // Reverse iterator before first

v.insert(pos, value);    // Insert at position (O(n))
v.erase(pos);             // Remove element at position (O(n))
```

---

## 🔹 `std::deque<T>` — Double-ended queue

```cpp
dq.push_back(x);         // Insert at end
dq.push_front(x);        // Insert at front
dq.pop_back();           // Remove from end
dq.pop_front();          // Remove from front

dq.front();              // Access front element
dq.back();               // Access last element

dq.size();               // Number of elements
dq.empty();              // Check if empty
```

---

## 🔹 `std::list<T>` — Doubly linked list

```cpp
lst.push_back(x);        // Insert at end
lst.push_front(x);       // Insert at front
lst.pop_back();          // Remove last
lst.pop_front();         // Remove first

lst.insert(pos, x);      // Insert before iterator (O(1))
lst.erase(pos);          // Remove element at iterator (O(1))

lst.remove(value);       // Remove all matching values
lst.remove_if(pred);     // Remove elements satisfying predicate

lst.sort();              // Sort list
lst.reverse();           // Reverse list
lst.unique();            // Remove consecutive duplicates
```

---

## 🔹 `std::forward_list<T>` — Singly linked list

```cpp
fl.push_front(x);        // Insert at front
fl.pop_front();          // Remove first element
fl.insert_after(pos, x); // Insert after iterator
fl.erase_after(pos);     // Remove element after iterator
```

---

# 📦 2. Associative Containers (Ordered — Tree Based)

---

## 🔹 `std::set<T>` — Unique, sorted keys

```cpp
s.insert(x);             // Insert element
s.emplace(x);            // Construct element in place
s.erase(x);              // Remove element
s.find(x);               // Find element (iterator or end)
s.count(x);              // Returns 0 or 1

s.lower_bound(x);        // First element >= x
s.upper_bound(x);        // First element > x

s.begin();               // Smallest element
s.end();                 // Past last element
```

---

## 🔹 `std::multiset<T>` — Sorted, duplicate keys allowed

```cpp
ms.insert(x);            // Insert element
ms.count(x);             // Count occurrences
ms.erase(x);             // Remove all matching elements
```

---

## 🔹 `std::map<K, V>` — Key-value pairs, sorted by key

```cpp
mp.insert({k, v});       // Insert key-value pair
mp.emplace(k, v);        // Construct key-value pair

mp[k];                   // Access value (inserts default if missing)
mp.at(k);                // Access value (throws if key missing)

mp.find(k);              // Find key
mp.erase(k);             // Remove key
mp.count(k);             // 0 or 1

mp.lower_bound(k);       // First key >= k
mp.upper_bound(k);       // First key > k
```

---

## 🔹 `std::multimap<K, V>` — Duplicate keys allowed

```cpp
mm.insert({k, v});       // Insert key-value pair
mm.find(k);              // Find first matching key
mm.equal_range(k);       // Range of matching keys
```

---

# 📦 3. Unordered Containers (Hash Based)

---

## 🔹 `std::unordered_set<T>`

```cpp
us.insert(x);            // Insert element
us.erase(x);             // Remove element
us.find(x);              // Find element
us.count(x);             // 0 or 1
```

---

## 🔹 `std::unordered_map<K, V>`

```cpp
um[k];                   // Access value (creates if missing)
um.at(k);                // Access value (throws if missing)

um.insert({k, v});       // Insert key-value pair
um.emplace(k, v);        // Construct key-value pair

um.erase(k);             // Remove key
um.find(k);              // Find key
```

---

# 📦 4. Container Adapters

---

## 🔹 `std::stack<T>` — LIFO

```cpp
st.push(x);              // Push element on top
st.emplace(x);           // Construct element on top
st.pop();                // Remove top element
st.top();                // Access top element
st.empty();              // Check if empty
st.size();               // Number of elements
```

---

## 🔹 `std::queue<T>` — FIFO

```cpp
q.push(x);               // Insert at back
q.emplace(x);            // Construct at back
q.pop();                 // Remove from front
q.front();               // Access front element
q.back();                // Access last element
q.empty();               // Check if empty
q.size();                // Number of elements
```

---

## 🔹 `std::priority_queue<T>` — Heap (Max by default)

```cpp
pq.push(x);              // Insert element
pq.emplace(x);           // Construct element in heap
pq.pop();                // Remove highest-priority element
pq.top();                // Access highest-priority element
pq.empty();              // Check if empty
pq.size();               // Number of elements
```

Min-heap:

```cpp
priority_queue<T, vector<T>, greater<T>> pq;
```

---

# 🔁 5. Iterators

```cpp
begin(), end();          // Forward traversal
cbegin(), cend();        // Const traversal
rbegin(), rend();        // Reverse traversal

advance(it, n);          // Move iterator forward by n
distance(a, b);          // Number of elements between iterators
```

---

# ⚙️ 6. Algorithms (`<algorithm>`)

---

## 🔹 Sorting / Rearranging

```cpp
sort(begin, end);                 // Sort range
sort(begin, end, comp);           // Sort with comparator
stable_sort(...);                 // Preserve relative order
reverse(...);                     // Reverse range
rotate(first, middle, last);      // Rotate elements
shuffle(...);                     // Random shuffle
```

---

## 🔹 Searching

```cpp
find(begin, end, x);               // Linear search
binary_search(begin, end, x);      // Binary search (sorted)

lower_bound(begin, end, x);        // First element >= x
upper_bound(begin, end, x);        // First element > x
equal_range(begin, end, x);        // Pair of bounds
```

---

## 🔹 Counting / Checking

```cpp
count(begin, end, x);              // Count value
count_if(begin, end, pred);        // Count by condition

any_of(...);                       // Any element matches
all_of(...);                       // All elements match
none_of(...);                      // No element matches
```

---

## 🔹 Min / Max

```cpp
min_element(...);                  // Iterator to minimum
max_element(...);                  // Iterator to maximum
minmax_element(...);               // Pair of min & max
```

---

## 🔹 Modifying

```cpp
copy(src_begin, src_end, dest);    // Copy elements
move(src_begin, src_end, dest);    // Move elements
fill(begin, end, x);               // Fill range
transform(...);                    // Apply function to range

remove(begin, end, x);             // Shift non-matching elements
remove_if(begin, end, pred);       // Shift by condition
unique(begin, end);                // Remove consecutive duplicates
```

⚠️ **Erase-Remove Idiom**

```cpp
v.erase(remove(v.begin(), v.end(), x), v.end());
```

---

# 🧮 7. Numeric Algorithms (`<numeric>`)

```cpp
accumulate(begin, end, init);      // Sum/reduce values
partial_sum(...);                  // Prefix sums
adjacent_difference(...);          // Differences between neighbors
iota(begin, end, start);           // Fill with increasing values
```

---

# 🧠 8. Functional (`<functional>`)

```cpp
std::function<ret(args)>;          // Type-erased callable
std::bind();                       // Bind arguments

std::plus<T>();                    // Addition functor
std::minus<T>();                   // Subtraction functor
std::greater<T>();                 // Greater-than comparator
std::less<T>();                    // Less-than comparator
```

---

# 🧩 9. Utilities (`<utility>`)

```cpp
std::pair<T1, T2>;                 // Pair of values
std::make_pair(a, b);              // Create pair

std::swap(a, b);                   // Swap values
std::move(x);                      // Cast to r-value
std::forward<T>(x);                // Perfect forwarding
```

---

# 🧠 10. Smart Pointers (`<memory>`)

---

## 🔹 `std::unique_ptr<T>`

```cpp
make_unique<T>();                  // Create unique ownership
reset();                            // Delete managed object
release();                          // Release ownership
```

---

## 🔹 `std::shared_ptr<T>`

```cpp
make_shared<T>();                  // Shared ownership
use_count();                        // Reference count
reset();                            // Release ownership
```

---

## 🔹 `std::weak_ptr<T>`

```cpp
lock();                             // Get shared_ptr if alive
expired();                          // Check if object destroyed
```

---

# ⏱️ 11. Time Utilities (`<chrono>`)

```cpp
steady_clock::now();               // Get current time
duration_cast<unit>(d);            // Convert time units
```

---

# 🚨 12. Exception Handling

```cpp
try { }                             // Protected block
catch (const std::exception& e) {   // Catch standard exception
    e.what();                       // Error message
}
```

---
