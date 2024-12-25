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

### 5.7.1.2.2 `std::map`

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

### 5.7.1.2.3 `std::multiset`

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

### 5.7.1.2.4 `std::multimap`

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

## 5.7.1.3 Unordered Containers

### 5.7.1.3.1 `std::unordered_set`

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

### 5.7.1.3.2 `std::unordered_map`

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

### 5.7.1.3.3 `std::unordered_multiset`

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

### 5.7.1.3.4 `std::unordered_multimap`

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
