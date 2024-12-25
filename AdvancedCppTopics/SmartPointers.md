# [5.4 Smart Pointers](#54-smart-pointers)

In C++, **smart pointers** are objects that manage the lifecycle of dynamically allocated memory. They ensure memory is automatically deallocated when it is no longer in use, helping prevent memory leaks and dangling pointers. Smart pointers reside in the `<memory>` header and are part of the C++ Standard Library.

This section explores the different types of smart pointers in C++:

- [5.4.1 Unique Pointer (`std::unique_ptr`)](#541-unique-pointer-stdunique_ptr)
- [5.4.2 Shared Pointer (`std::shared_ptr`)](#542-shared-pointer-stdshared_ptr)
- [5.4.3 Weak Pointer (`std::weak_ptr`)](#543-weak-pointer-stdweak_ptr)
- [5.4.4 Auto Pointer (`std::auto_ptr`) (Deprecated)](#544-auto-pointer-stdauto_ptr-deprecated)

---

## [5.4.1 Unique Pointer (`std::unique_ptr`)](#541-unique-pointer-stdunique_ptr)

A **unique pointer** is a smart pointer that **owns and manages a dynamically allocated object**. It ensures that only one `std::unique_ptr` object can own the resource at any given time. When the `std::unique_ptr` is destroyed, the resource is automatically deallocated.

**Key Features:**
- Transfer ownership using `std::move`.
- Non-copyable to ensure unique ownership.

**Example:**
```cpp
#include <iostream>
#include <memory>
using namespace std;

int main() {
    unique_ptr<int> uptr = make_unique<int>(10);
    cout << "Unique Pointer value: " << *uptr << endl;
    // Output: Unique Pointer value: 10

    // uptr2 = uptr; // Error: unique_ptr cannot be copied
    unique_ptr<int> uptr2 = move(uptr);
    cout << "Unique Pointer value after move: " << *uptr2 << endl;
    // Output: Unique Pointer value after move: 10

    return 0;
}
```

**Use Case:** 
- Use `std::unique_ptr` when a resource needs strict ownership without sharing.

---

## [5.4.2 Shared Pointer (`std::shared_ptr`)](#542-shared-pointer-stdshared_ptr)

A **shared pointer** is a smart pointer that **allows multiple shared ownership** of the same dynamically allocated object. The object is destroyed when the last `std::shared_ptr` that owns it is destroyed or reset.

**Key Features:**
- Reference counting is used to manage ownership.
- Thread-safe reference count management.

**Example:**
```cpp
#include <iostream>
#include <memory>
using namespace std;

int main() {
    shared_ptr<int> sptr1 = make_shared<int>(20);
    shared_ptr<int> sptr2 = sptr1; // Shared ownership
    cout << "Shared Pointer value: " << *sptr1 << endl;
    // Output: Shared Pointer value: 20
    cout << "Shared Pointer use count: " << sptr1.use_count() << endl;
    // Output: Shared Pointer use count: 2

    sptr2.reset(); // Release one ownership
    cout << "Shared Pointer use count after reset: " << sptr1.use_count() << endl;
    // Output: Shared Pointer use count after reset: 1

    return 0;
}
```

**Use Case:**
- Use `std::shared_ptr` when multiple owners of a resource are required.

---

## [5.4.3 Weak Pointer (`std::weak_ptr`)](#543-weak-pointer-stdweak_ptr)

A **weak pointer** is a smart pointer that **does not own the resource** it points to. It is used to prevent **cyclic dependencies** between `std::shared_ptr` instances, which can cause memory leaks.

**Key Features:**
- Observes a `std::shared_ptr` without increasing its reference count.
- Can be promoted to `std::shared_ptr` using `lock()`.

**Example:**
```cpp
#include <iostream>
#include <memory>
using namespace std;

int main() {
    shared_ptr<int> sptr = make_shared<int>(30);
    weak_ptr<int> wptr = sptr;

    cout << "Weak Pointer use count: " << wptr.use_count() << endl;
    // Output: Weak Pointer use count: 1
    if (auto spt = wptr.lock()) { // Check if shared_ptr is valid
        cout << "Weak Pointer value: " << *spt << endl;
        // Output: Weak Pointer value: 30
    } else {
        cout << "Weak Pointer is expired" << endl;
    }

    sptr.reset(); // Release shared_ptr ownership
    if (wptr.expired()) {
        cout << "Weak Pointer is expired after reset" << endl;
        // Output: Weak Pointer is expired after reset
    }

    return 0;
}

```

**Use Case:**
- Use `std::weak_ptr` to break cycles in shared ownership (e.g., in graph-like structures).

---

## [5.4.4 Auto Pointer (`std::auto_ptr`) (Deprecated)](#544-auto-pointer-stdauto_ptr-deprecated)

The **auto pointer** was an early smart pointer in C++ but has been deprecated in C++11 and removed in C++17. It is replaced by `std::unique_ptr`.

**Key Features:**
- Automatically deletes the object when `std::auto_ptr` goes out of scope.
- Non-intuitive ownership transfer semantics (ownership is transferred on copy).

**Example:**
```cpp
#include <iostream>
#include <memory>
using namespace std;

int main() {
    auto_ptr<int> aptr1(new int(40));
    cout << "Auto Pointer value: " << *aptr1 << endl;
    // Output: Auto Pointer value: 40

    auto_ptr<int> aptr2 = aptr1; // Ownership is transferred
    // cout << *aptr1; // Undefined behavior (ownership lost)
    cout << "Auto Pointer value after transfer: " << *aptr2 << endl;
    // Output: Auto Pointer value after transfer: 40

    return 0;
}
```

**Why Deprecated:**
- Ownership transfer semantics are unintuitive.
- `std::unique_ptr` provides a safer and more robust alternative.

---

### [Summary](#summary)

| **Smart Pointer**    | **Ownership**         | **Key Features**                                           | **Use Cases**                                          |
|-----------------------|-----------------------|-----------------------------------------------------------|-------------------------------------------------------|
| `std::unique_ptr`     | Single ownership     | Non-copyable, transfers ownership with `std::move`.       | Strict ownership with no sharing.                    |
| `std::shared_ptr`     | Shared ownership     | Reference counting, shared ownership.                     | Resource shared by multiple owners.                  |
| `std::weak_ptr`       | Observes ownership   | Breaks cycles in `std::shared_ptr`, uses `lock()` to access. | Prevent cyclic dependencies in shared ownership.      |
| `std::auto_ptr`       | Deprecated           | Ownership transfer on copy.                               | Avoid; replaced by `std::unique_ptr`.                |

Smart pointers are essential tools in modern C++ programming, offering safety and convenience when managing dynamic memory. They help prevent common pitfalls such as memory leaks and dangling pointers. Always choose the right smart pointer based on your use case for effective resource management.

---
