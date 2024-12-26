# [5.7 Standard Template Library (STL)](#57-standard-template-library-stl)
The Standard Template Library (STL) is a powerful library in C++ that provides a collection of template classes and functions designed to make working with data structures and algorithms easier and more efficient. STL includes containers, iterators, algorithms, and function objects (functors).

---


## [5.7.1.1 Sequential Containers](#5711-sequential-containers)

### [5.7.1.1.1 `std::vector`](#57111-stdvector)

**Description:** A `std::vector` is a dynamic array that can grow or shrink in size. It provides fast random access to elements.

**Characteristics:**
- **Access Time:** O(1) for accessing elements by index.
- **Insertions/Deletions:** Inserting/removing elements at the end is fast (O(1)), but inserting/removing elements at the beginning or in the middle is slow (O(n)).
- **Memory:** Contiguous memory layout, which is cache-friendly.

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    // Initialize vector
    vector<int> v = {1, 2, 3};

    // Adding elements to the vector
    v.push_back(4);  // Add to the end
    v.insert(v.begin(), 0);  // Insert at the beginning
    v.erase(v.begin() + 1);  // Remove element at index 1

    // Accessing elements
    cout << "First element: " << v[0] << endl; // Accessing using index
    cout << "Size of vector: " << v.size() << endl;  // Size of the vector
    
    // Iterating over the vector
    cout << "Vector elements: ";
    for (int num : v) {
        cout << num << " ";  // Output: 0 3 4
    }
    cout << endl;

    return 0;
}

// Output:
// First element: 0
// Size of vector: 3
// Vector elements: 0 3 4
```

### [5.7.1.1.2 `std::deque`](#57112-stddeque)

**Description:** A `std::deque` (double-ended queue) allows fast insertions and deletions at both ends.

**Characteristics:**
- Access Time: O(1) for accessing elements by index.
- Insertions/Deletions: O(1) for inserting or deleting elements at both ends, but O(n) for inserting/removing in the middle.
- Memory: Non-contiguous memory layout.


```cpp
#include <iostream>
#include <deque>
using namespace std;

int main() {
    // Initialize deque
    deque<int> d = {1, 2, 3};

    // Adding elements
    d.push_back(4);  // Add to the end
    d.push_front(0);  // Add to the front
    d.pop_back();  // Remove element from the end
    d.pop_front();  // Remove element from the front

    // Accessing elements
    cout << "First element: " << d.front() << endl;  // First element
    cout << "Last element: " << d.back() << endl;  // Last element
    cout << "Size of deque: " << d.size() << endl;

    // Iterating over the deque
    cout << "Deque elements: ";
    for (int num : d) {
        cout << num << " ";  // Output: 1 2
    }
    cout << endl;

    return 0;
}

// Output:
// First element: 1
// Last element: 2
// Size of deque: 2
// Deque elements: 1 2
```

### [5.7.1.1.3 `std::list`](#57113-stdlist)

**Description:** A `std::list` is a doubly linked list that allows efficient insertions and deletions from both ends and in the middle.

**Characteristics:**
- **Access Time:** O(n) for accessing elements by index (since it is not contiguous).
- **Insertions/Deletions:** O(1) for inserting/removing elements anywhere in the list (given an iterator).
- **Memory:** Each element is linked to the next, so it is non-contiguous in memory.

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    // Initialize list
    list<int> l = {1, 2, 3};

    // Adding elements
    l.push_back(4);  // Add to the end
    l.push_front(0);  // Add to the front
    l.pop_back();  // Remove element from the end

    // Accessing elements (using iterators)
    cout << "First element: " << l.front() << endl;
    cout << "Last element: " << l.back() << endl;

    // Iterating over the list
    cout << "List elements: ";
    for (int num : l) {
        cout << num << " ";  // Output: 0 1 2
    }
    cout << endl;

    return 0;
}

// Output:
// First element: 0
// Last element: 2
// List elements: 0 1 2
```

---

## [5.7.1.2 Associative Containers](#5712-associative-containers)
Associative containers store elements in key-value pairs and automatically order the elements by their keys. They provide fast lookup, insertion, and deletion based on the key.


### [5.7.1.2.1 `std::set`](#57121-stdset)
**Description:** A `std::set` is a collection of unique elements, automatically ordered by their value.

**Characteristics:**
- **Access Time:** O(log n) for searching, insertion, and deletion.
- **Order:** Elements are automatically sorted.
- **Duplicates:** Does not allow duplicate values.

```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    // Initialize set
    set<int> s = {3, 1, 2};

    // Adding elements
    s.insert(4);  // Add element
    s.insert(2);  // Duplicate elements are ignored

    // Searching for an element
    if (s.find(3) != s.end()) {
        cout << "Element 3 found in set." << endl;
    }

    // Iterating over the set
    cout << "Set elements: ";
    for (int num : s) {
        cout << num << " ";  // Output: 1 2 3 4 (automatically sorted)
    }
    cout << endl;

    return 0;
}

// Output:
// Element 3 found in set.
// Set elements: 1 2 3 4
```

### [5.7.1.2.2 `std::map`](#57122-stdmap)

**Description:** A `std::map` is a collection of key-value pairs where keys are unique and are stored in a sorted order.

**Characteristics:**
- **Access Time:** O(log n) for searching, insertion, and deletion.
- **Order:** Keys are sorted automatically.
- **Duplicates:** Does not allow duplicate keys.

```cpp
#include <iostream>
#include <map>
using namespace std;

int main() {
    // Initialize map
    map<string, int> m = {{"Alice", 25}, {"Bob", 30}};

    // Adding elements
    m["Charlie"] = 35;  // Insert or update element

    // Searching for a key
    if (m.find("Bob") != m.end()) {
        cout << "Bob's age is " << m["Bob"] << endl;
    }

    // Iterating over the map
    cout << "Map elements:\n";
    for (auto &p : m) {
        cout << p.first << ": " << p.second << endl;
    }
    return 0;
}

// Output:
// Bob's age is 30
// Map elements:
// Alice: 25
// Bob: 30
// Charlie: 35
```

### [5.7.1.2.3 `std::multiset`](#57123-stdmultiset)

**Description:** A `std::multiset` is similar to `std::set`, but it allows duplicate elements.

**Characteristics:**
- **Access Time:** O(log n) for searching, insertion, and deletion.
- **Order:** Elements are automatically sorted.
- **Duplicates:** Allows duplicate values.

```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    // Initialize multiset
    multiset<int> ms = {1, 2, 2, 3};

    // Adding duplicate elements
    ms.insert(2);  // Duplicate elements allowed

    // Iterating over the multiset
    cout << "Multiset elements: ";
    for (int num : ms) {
        cout << num << " ";  // Output: 1 2 2 2 3
    }
    cout << endl;

    return 0;
}

// Output:
// Multiset elements: 1 2 2 2 3
```

### [5.7.1.2.4 `std::multimap`](#57124-stdmultimap)

**Description:** A `std::multimap` is similar to `std::map`, but it allows duplicate keys.

**Characteristics:**
- **Access Time:** O(log n) for searching, insertion, and deletion.
- **Order:** Keys are sorted automatically.
- **Duplicates:** Allows duplicate keys.

```cpp
#include <iostream>
#include <map>
using namespace std;

int main() {
    // Initialize multimap
    multimap<string, int> mm = {{"Alice", 25}, {"Bob", 30}, {"Alice", 28}};

    // Adding duplicate keys
    mm.insert({"Alice", 22});

    // Iterating over the multimap
    cout << "Multimap elements:\n";
    for (auto &p : mm) {
        cout << p.first << ": " << p.second << endl;  // Output: Alice: 25, Alice: 28, Alice: 22, Bob: 30
    }

    return 0;
}

// Output:
// Multimap elements:
// Alice: 25
// Alice: 28
// Alice: 22
// Bob: 30
```

---

## [5.7.1.3 Unordered Containers](#5713-unordered-containers)

Unordered containers store elements in an unordered fashion, usually for faster average time complexity for lookup, insertion, and deletion operations (using hash tables).

### [5.7.1.3.1 `std::unordered_set`](#57131-stdunordered_set)

**Description:** A `std::unordered_set` is a collection of unique elements stored without any specific order.

**Characteristics:**
- **Access Time:** O(1) on average for searching, insertion, and deletion.
- **Order:** No guaranteed order.
- **Duplicates:** Does not allow duplicate values.

```cpp
#include <iostream>
#include <unordered_set>
using namespace std;

int main() {
    // Initialize unordered set
    unordered_set<int> us = {3, 1, 2};

    // Adding elements
    us.insert(4);

    // Searching for an element
    if (us.find(3) != us.end()) {
        cout << "Element 3 found in unordered set." << endl;
    }

    // Iterating over the unordered set
    cout << "Unordered set elements: ";
    for (int num : us) {
        cout << num << " ";  // Output order may vary, e.g., 1 2 3 4
    }
    cout << endl;

    return 0;
}

// Output (order may vary):
// Element 3 found in unordered set.
// Unordered set elements: 1 2 3 4
```

### [5.7.1.3.2 `std::unordered_map`](#57132-stdunordered_map)

**Description:** A std::unordered_map is a collection of key-value pairs, where keys are unique and stored in an unordered fashion.

**Characteristics:**
- **Access Time:** O(1) on average for searching, insertion, and deletion.
- **Order:** No guaranteed order.
- **Duplicates:** Does not allow duplicate keys.

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

int main() {
    // Initialize unordered map
    unordered_map<string, int> um = {{"Alice", 25}, {"Bob", 30}};

    // Adding elements
    um["Charlie"] = 35;

    // Searching for a key
    if (um.find("Bob") != um.end()) {
        cout << "Bob's age is " << um["Bob"] << endl;
    }

    // Iterating over the unordered map
    cout << "Unordered map elements:\n";
    for (auto &p : um) {
        cout << p.first << ": " << p.second << endl;
    }

    return 0;
}

// Output (order may vary):
// Bob's age is 30
// Unordered map elements:
// Alice: 25
// Bob: 30
// Charlie: 35
```

### [5.7.1.3.3 `std::unordered_multiset`](#57133-stdunordered_multiset)

**Description:** A std::unordered_multiset allows duplicate elements but does not guarantee any specific order.

**Characteristics:**
- **Access Time:** O(1) on average for searching, insertion, and deletion.
- **Order:** No guaranteed order.
- **Duplicates:** Allows duplicate values.

```cpp
#include <iostream>
#include <unordered_set>
using namespace std;

int main() {
    // Initialize unordered multiset
    unordered_multiset<int> ums = {1, 2, 2, 3};

    // Adding duplicate elements
    ums.insert(2);

    // Iterating over the unordered multiset
    cout << "Unordered multiset elements: ";
    for (int num : ums) {
        cout << num << " ";  // Output order may vary, e.g., 1 2 2 2 3
   

 }
    cout << endl;

    return 0;
}

// Output (order may vary):
// Unordered multiset elements: 1 2 2 2 3
```

### [5.7.1.3.4 `std::unordered_multimap`](#57134-stdunordered_multimap)

**Description:** A std::unordered_multimap is similar to std::unordered_map, but it allows duplicate keys.

**Characteristics:**
- **Access Time:** O(1) on average for searching, insertion, and deletion.
- **Order:** No guaranteed order.
- **Duplicates:** Allows duplicate keys.

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

int main() {
    // Initialize unordered multimap
    unordered_multimap<string, int> umm = {{"Alice", 25}, {"Bob", 30}, {"Alice", 28}};

    // Adding duplicate keys
    umm.insert({"Alice", 22});

    // Iterating over the unordered multimap
    cout << "Unordered multimap elements:\n";
    for (auto &p : umm) {
        cout << p.first << ": " << p.second << endl;  // Output: Alice: 25, Alice: 28, Alice: 22, Bob: 30
    }

    return 0;
}

// Output (order may vary):
// Unordered multimap elements:
// Alice: 25
// Alice: 28
// Alice: 22
// Bob: 30
```

---


## [5.7.2 Iterators](#572-iterators)

Iterators in C++ are objects that enable you to traverse through the elements of a container (like vectors, sets, or maps). They are similar to pointers and allow you to read or modify elements in the container. C++ provides several types of iterators, each suited to different use cases.

---

### [5.7.2.1 Input Iterators](#5721-input-iterators)

Input iterators are used for reading the elements of a container one at a time. They only allow access to each element once in a single direction, typically from the beginning to the end.

**Key properties of input iterators**:
- Can only be used to read values (i.e., they provide "read-only" access).
- They only support a one-way traversal, i.e., you can only move forward.

**Example**:

```cpp
#include <iostream>
#include <vector>
#include <iterator>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};

    // Input iterator example using begin and end
    vector<int>::iterator it = vec.begin();
    cout << "Input Iterator: ";
    while (it != vec.end()) {
        cout << *it << " "; // Output: 1 2 3 4 5
        ++it; // Move to the next element
    }
    cout << endl;

    return 0;
}

// Output:
// Input Iterator: 1 2 3 4 5
```

In the example, the iterator `it` traverses through the vector `vec` and outputs each element. The movement of the iterator is only forward.

---

### [5.7.2.2 Output Iterators](#5722-output-iterators)

Output iterators are used for writing data to a container. You can only assign a value to the location pointed to by the iterator, and you can move forward. They are typically used in scenarios like output streams or modifying containers.

**Key properties of output iterators**:
- Can only be used for writing data.
- They only support one-way traversal (forward).

**Example**:

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> vec(5);  // Create a vector with 5 elements

    // Output iterator example using begin
    vector<int>::iterator it = vec.begin();
    cout << "Output Iterator: ";
    for (int i = 0; i < 5; ++i) {
        *it = i + 1;  // Assign values to the vector
        cout << *it << " "; // Output: 1 2 3 4 5
        ++it;  // Move to the next element
    }
    cout << endl;

    return 0;
}

// Output:
// Output Iterator: 1 2 3 4 5
```

Here, the output iterator `it` is used to write values into the `vec` container, moving through the container from beginning to end.

---

### [5.7.2.3 Forward Iterators](#5723-forward-iterators)

Forward iterators can read or write to the container and can traverse forward. These iterators support reading and modifying elements and can traverse through the container multiple times in a single direction.

**Key properties of forward iterators**:
- Can be used for both reading and writing elements.
- They allow forward traversal and can revisit elements if necessary.

**Example**:

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    list<int> l = {1, 2, 3, 4, 5};

    // Forward iterator example using begin and end
    list<int>::iterator it = l.begin();
    cout << "Forward Iterator: ";
    while (it != l.end()) {
        cout << *it << " ";  // Output: 1 2 3 4 5
        ++it;  // Move to the next element
    }
    cout << endl;

    return 0;
}

// Output:
// Forward Iterator: 1 2 3 4 5
```

The forward iterator `it` traverses through the list `l`, allowing both reading and moving forward through the elements.

---

### [5.7.2.4 Bidirectional Iterators](#5724-bidirectional-iterators)

Bidirectional iterators can traverse a container both forward and backward. They provide more flexibility than forward iterators as they allow reverse iteration.

**Key properties of bidirectional iterators**:
- Can read and write values.
- Can move both forward and backward through the container.
- Commonly used with containers like `std::list` that support both directions of traversal.

**Example**:

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    list<int> l = {1, 2, 3, 4, 5};

    // Bidirectional iterator example using begin and rbegin
    list<int>::iterator it = l.begin();
    cout << "Bidirectional Iterator (forward): ";
    while (it != l.end()) {
        cout << *it << " ";  // Output: 1 2 3 4 5
        ++it;  // Move forward
    }
    cout << endl;

    // Moving backward with reverse iterator
    list<int>::reverse_iterator rit = l.rbegin();
    cout << "Bidirectional Iterator (reverse): ";
    while (rit != l.rend()) {
        cout << *rit << " ";  // Output: 5 4 3 2 1
        ++rit;  // Move backward
    }
    cout << endl;

    return 0;
}

// Output:
// Bidirectional Iterator (forward): 1 2 3 4 5
// Bidirectional Iterator (reverse): 5 4 3 2 1
```

In this example, the bidirectional iterator `it` moves forward, and the reverse iterator `rit` moves backward through the `list` container.

---

### [5.7.2.5 Random Access Iterators](#5725-random-access-iterators)

Random access iterators allow you to access elements at any position in the container and can move in any direction, both forward and backward. They are typically used with containers like `std::vector` and `std::deque`, where elements are stored in contiguous memory.

**Key properties of random access iterators**:
- Can read and write values.
- Supports moving both forward and backward with constant time complexity (O(1)).
- Allows accessing elements directly by index.

**Example**:

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> vec = {10, 20, 30, 40, 50};

    // Random access iterator example using begin and end
    vector<int>::iterator it = vec.begin();
    cout << "Random Access Iterator (forward): ";
    for (int i = 0; i < vec.size(); ++i) {
        cout << *(it + i) << " ";  // Output: 10 20 30 40 50 (direct access by index)
    }
    cout << endl;

    // Reverse iteration using random access iterator
    cout << "Random Access Iterator (reverse): ";
    for (int i = vec.size() - 1; i >= 0; --i) {
        cout << *(it + i) << " ";  // Output: 50 40 30 20 10 (direct access by index)
    }
    cout << endl;

    return 0;
}

// Output:
// Random Access Iterator (forward): 10 20 30 40 50
// Random Access Iterator (reverse): 50 40 30 20 10
```

In this example, the random access iterator `it` provides direct access to elements in both forward and reverse directions, allowing for efficient random access.

---

## Summary of Iterator Types:

- **Input Iterators**: Can only read values, move forward. (`vector<int>::iterator`)
- **Output Iterators**: Can only write values, move forward. (`vector<int>::iterator`)
- **Forward Iterators**: Can read and write, move forward, and revisit elements. (`list<int>::iterator`)
- **Bidirectional Iterators**: Can read and write, move forward and backward. (`list<int>::iterator`)
- **Random Access Iterators**: Can read and write, move forward and backward, and access elements by index in constant time. (`vector<int>::iterator`)

---

## [5.7.3 Algorithms](#573-algorithms)

The `<algorithm>` header in C++ provides a variety of algorithms to perform operations on data structures. These algorithms simplify common tasks such as searching, sorting, manipulating data, and performing numeric computations.

---

### **[5.7.3.1 Searching Algorithms](#5731-searching-algorithms)**

Searching algorithms in C++ are used to find specific elements in containers. The Standard Library provides functions like `std::find`, `std::binary_search`, etc.

**Example: Searching in a container**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> vec = {10, 20, 30, 40, 50};

    // Linear search using std::find
    auto it = find(vec.begin(), vec.end(), 30);
    if (it != vec.end()) {
        cout << "Element found: " << *it << endl;  // Output: Element found: 30
    } else {
        cout << "Element not found" << endl;
    }

    // Binary search (requires sorted data)
    bool found = binary_search(vec.begin(), vec.end(), 40);
    cout << "Binary search result for 40: " << (found ? "Found" : "Not Found") << endl;  // Output: Found

    return 0;
}

// Output:
// Element found: 30
// Binary search result for 40: Found
```

---

### **[5.7.3.2 Sorting Algorithms](#5732-sorting-algorithms)**

Sorting algorithms reorder the elements in a container. The Standard Library provides functions like `std::sort`, `std::stable_sort`, and `std::partial_sort`.

**Example: Sorting a container**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> vec = {50, 10, 40, 20, 30};

    // Sorting in ascending order
    sort(vec.begin(), vec.end());
    cout << "Sorted (ascending): ";
    for (int val : vec) cout << val << " ";  // Output: 10 20 30 40 50
    cout << endl;

    // Sorting in descending order
    sort(vec.rbegin(), vec.rend());
    cout << "Sorted (descending): ";
    for (int val : vec) cout << val << " ";  // Output: 50 40 30 20 10
    cout << endl;

    return 0;
}

// Output:
// Sorted (ascending): 10 20 30 40 50
// Sorted (descending): 50 40 30 20 10
```

---

### **[5.7.3.3 Manipulation Algorithms](#5733-manipulation-algorithms)**

Manipulation algorithms perform operations like reversing, rotating, or removing elements in a container. Examples include `std::reverse`, `std::rotate`, and `std::remove`.

**Example: Manipulating a container**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};

    // Reversing a container
    reverse(vec.begin(), vec.end());
    cout << "Reversed: ";
    for (int val : vec) cout << val << " ";  // Output: 5 4 3 2 1
    cout << endl;

    // Rotating a container
    rotate(vec.begin(), vec.begin() + 2, vec.end());
    cout << "Rotated: ";
    for (int val : vec) cout << val << " ";  // Output: 3 2 1 5 4
    cout << endl;

    // Removing an element
    vec.erase(remove(vec.begin(), vec.end(), 3), vec.end());
    cout << "After removing 3: ";
    for (int val : vec) cout << val << " ";  // Output: 2 1 5 4
    cout << endl;

    return 0;
}

// Output:
// Reversed: 5 4 3 2 1
// Rotated: 3 2 1 5 4
// After removing 3: 2 1 5 4
```

---

### **[5.7.3.4 Numeric Algorithms](#5734-numeric-algorithms)**

Numeric algorithms perform operations like summing elements, calculating partial sums, or generating products. Examples include `std::accumulate`, `std::partial_sum`, and `std::iota`.

**Example: Numeric operations on a container**

```cpp
#include <iostream>
#include <vector>
#include <numeric>
using namespace std;

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};

    // Summing all elements
    int sum = accumulate(vec.begin(), vec.end(), 0);
    cout << "Sum: " << sum << endl;  // Output: Sum: 15

    // Calculating partial sums
    vector<int> partialSums(vec.size());
    partial_sum(vec.begin(), vec.end(), partialSums.begin());
    cout << "Partial sums: ";
    for (int val : partialSums) cout << val << " ";  // Output: 1 3 6 10 15
    cout << endl;

    // Generating sequential values
    vector<int> seq(5);
    iota(seq.begin(), seq.end(), 10);  // Start from 10
    cout << "Sequential values: ";
    for (int val : seq) cout << val << " ";  // Output: 10 11 12 13 14
    cout << endl;

    return 0;
}

// Output:
// Sum: 15
// Partial sums: 1 3 6 10 15
// Sequential values: 10 11 12 13 14
```

---

## Summary

- **[5.7.3.1 Searching Algorithms](#5731-searching-algorithms)**: Locate elements in a container (e.g., `std::find`, `std::binary_search`).
- **[5.7.3.2 Sorting Algorithms](#5732-sorting-algorithms)**: Reorder elements (e.g., `std::sort`, `std::stable_sort`).
- **[5.7.3.3 Manipulation Algorithms](#5733-manipulation-algorithms)**: Modify containers (e.g., `std::reverse`, `std::rotate`, `std::remove`).
- **[5.7.3.4 Numeric Algorithms](#5734-numeric-algorithms)**: Perform numeric operations (e.g., `std::accumulate`, `std::partial_sum`, `std::iota`).

Each algorithm can significantly simplify operations on data structures and improve code efficiency.

---

## [5.7.4 Function Objects (Functors)](#574-function-objects)

A **function object** or **functor** is any object that can be called using the function call operator `()`. Functors are widely used in C++ with algorithms and STL containers, as they enable encapsulation of custom logic and state within an object that behaves like a function.

---

### **[5.7.4.1 Built-in Functors](#5741-built-in-functors)**

C++ provides several predefined functors in the `<functional>` header. These functors are used to perform operations like comparisons, arithmetic, logical operations, etc.

**Common Built-in Functors**
- `std::plus`: Adds two values.
- `std::minus`: Subtracts one value from another.
- `std::multiplies`: Multiplies two values.
- `std::less`: Compares two values to check if one is less than the other.
- `std::greater`: Compares two values to check if one is greater than the other.

**Example: Using built-in functors**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>
using namespace std;

int main() {
    vector<int> vec = {10, 20, 30, 40};

    // Using std::plus
    int sum = accumulate(vec.begin(), vec.end(), 0, plus<int>());
    cout << "Sum using std::plus: " << sum << endl;  // Output: Sum using std::plus: 100

    // Using std::less with sort
    sort(vec.begin(), vec.end(), less<int>());
    cout << "Sorted (ascending) using std::less: ";
    for (int val : vec) cout << val << " ";  // Output: 10 20 30 40
    cout << endl;

    // Using std::greater with sort
    sort(vec.begin(), vec.end(), greater<int>());
    cout << "Sorted (descending) using std::greater: ";
    for (int val : vec) cout << val << " ";  // Output: 40 30 20 10
    cout << endl;

    return 0;
}

// Output:
// Sum using std::plus: 100
// Sorted (ascending) using std::less: 10 20 30 40
// Sorted (descending) using std::greater: 40 30 20 10
```

---

### **[5.7.4.2 Custom Functors](#5742-custom-functors)**

Custom functors are user-defined classes that overload the `()` operator. They allow you to define custom behavior to use with algorithms or other contexts.

**Example: A custom functor to calculate squares**

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

// Custom functor
class Square {
public:
    int operator()(int x) const {
        return x * x;
    }
};

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};

    // Transform each element using the custom functor
    vector<int> result(vec.size());
    transform(vec.begin(), vec.end(), result.begin(), Square());

    cout << "Squares of elements: ";
    for (int val : result) cout << val << " ";  // Output: 1 4 9 16 25
    cout << endl;

    return 0;
}

// Output:
// Squares of elements: 1 4 9 16 25
```

**Example: A functor with state**

A functor can also have internal state to influence its behavior.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

// Custom functor with state
class Adder {
    int increment;
public:
    Adder(int inc) : increment(inc) {}
    int operator()(int x) const {
        return x + increment;
    }
};

int main() {
    vector<int> vec = {1, 2, 3, 4, 5};

    // Use Adder functor to add 10 to each element
    vector<int> result(vec.size());
    transform(vec.begin(), vec.end(), result.begin(), Adder(10));

    cout << "After adding 10 to each element: ";
    for (int val : result) cout << val << " ";  // Output: 11 12 13 14 15
    cout << endl;

    return 0;
}

// Output:
// After adding 10 to each element: 11 12 13 14 15
```

---

## Summary

- **[5.7.4.1 Built-in Functors](#5741-built-in-functors)**:
  Predefined functors in the `<functional>` header simplify common operations like addition, subtraction, comparison, etc.

- **[5.7.4.2 Custom Functors](#5742-custom-functors)**:
  User-defined classes can implement custom logic by overloading the `()` operator. They are useful for more complex operations or when state is required.

Functors provide a flexible and reusable way to encapsulate logic, making them essential tools for working with algorithms and containers in C++.

---



