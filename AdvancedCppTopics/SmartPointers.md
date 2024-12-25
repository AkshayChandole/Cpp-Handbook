# [5.4 Smart Pointers](#54-smart-pointers)

In C++, **smart pointers** are objects that manage the lifecycle of dynamically allocated memory. They ensure memory is automatically deallocated when it is no longer in use, helping prevent memory leaks and dangling pointers. Smart pointers reside in the `<memory>` header and are part of the C++ Standard Library.

This section explores the different types of smart pointers in C++:

- [5.4.1 Unique Pointer (`std::unique_ptr`)](#541-unique-pointer-stduniqueptr)
- [5.4.2 Shared Pointer (`std::shared_ptr`)](#542-shared-pointer-stdsharedptr)
- [5.4.3 Weak Pointer (`std::weak_ptr`)](#543-weak-pointer-stdweakptr)
- [5.4.4 Auto Pointer (`std::auto_ptr`) (Deprecated)](#544-auto-pointer-stdautoptr-deprecated)

---

## [5.4.1 Unique Pointer (`std::unique_ptr`)](#541-unique-pointer-stduniqueptr)

A **unique pointer** is a smart pointer that **owns and manages a dynamically allocated object**. It ensures that only one `std::unique_ptr` object can own the resource at any given time. When the `std::unique_ptr` is destroyed, the resource is automatically deallocated.

**Key Features:**
- Transfer ownership using `std::move`.
- Non-copyable to ensure unique ownership.

**Example:**
```cpp
#include <iostream>
#include <memory>

int main() {
    std::unique_ptr<int> ptr = std::make_unique<int>(42);
    std::cout << "Value: " << *ptr << std::endl;

    // Transfer ownership
    std::unique_ptr<int> newPtr = std::move(ptr);
    if (!ptr) {
        std::cout << "Ownership transferred." << std::endl;
    }

    return 0;
}
```

**Use Case:** 
- Use `std::unique_ptr` when a resource needs strict ownership without sharing.

---

## [5.4.2 Shared Pointer (`std::shared_ptr`)](#542-shared-pointer-stdsharedptr)

A **shared pointer** is a smart pointer that **allows multiple shared ownership** of the same dynamically allocated object. The object is destroyed when the last `std::shared_ptr` that owns it is destroyed or reset.

**Key Features:**
- Reference counting is used to manage ownership.
- Thread-safe reference count management.

**Example:**
```cpp
#include <iostream>
#include <memory>

int main() {
    std::shared_ptr<int> ptr1 = std::make_shared<int>(42);
    std::shared_ptr<int> ptr2 = ptr1; // Shared ownership

    std::cout << "Value: " << *ptr1 << ", Use Count: " << ptr1.use_count() << std::endl;

    return 0;
}
```

**Use Case:**
- Use `std::shared_ptr` when multiple owners of a resource are required.

---

## [5.4.3 Weak Pointer (`std::weak_ptr`)](#543-weak-pointer-stdweakptr)

A **weak pointer** is a smart pointer that **does not own the resource** it points to. It is used to prevent **cyclic dependencies** between `std::shared_ptr` instances, which can cause memory leaks.

**Key Features:**
- Observes a `std::shared_ptr` without increasing its reference count.
- Can be promoted to `std::shared_ptr` using `lock()`.

**Example:**
```cpp
#include <iostream>
#include <memory>

int main() {
    std::shared_ptr<int> shared = std::make_shared<int>(42);
    std::weak_ptr<int> weak = shared; // Observes shared pointer

    if (auto locked = weak.lock()) {
        std::cout << "Value: " << *locked << std::endl;
    } else {
        std::cout << "Resource is no longer available." << std::endl;
    }

    return 0;
}
```

**Use Case:**
- Use `std::weak_ptr` to break cycles in shared ownership (e.g., in graph-like structures).

---

## [5.4.4 Auto Pointer (`std::auto_ptr`) (Deprecated)](#544-auto-pointer-stdautoptr-deprecated)

The **auto pointer** was an early smart pointer in C++ but has been deprecated in C++11 and removed in C++17. It is replaced by `std::unique_ptr`.

**Key Features:**
- Automatically deletes the object when `std::auto_ptr` goes out of scope.
- Non-intuitive ownership transfer semantics (ownership is transferred on copy).

**Example:**
```cpp
#include <iostream>
#include <memory>

int main() {
    // Deprecated example
    std::auto_ptr<int> ptr(new int(42));
    std::cout << "Value: " << *ptr << std::endl;
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
