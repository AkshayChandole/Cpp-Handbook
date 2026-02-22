# [5.4 Smart Pointers](#54-smart-pointers)

In C++, **smart pointers** are objects that manage the lifecycle of dynamically allocated memory. They ensure memory is automatically deallocated when it is no longer in use, helping prevent memory leaks and dangling pointers. Smart pointers reside in the `<memory>` header and are part of the C++ Standard Library.

This section explores the different types of smart pointers in C++:

- [5.4.1 Unique Pointer (`std::unique_ptr`)](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/UniquePointer.md#unique-pointer)
- [5.4.2 Shared Pointer (`std::shared_ptr`)](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/SharedPointer.md#shared-pointer)
- [5.4.3 Weak Pointer (`std::weak_ptr`)](https://github.com/AkshayChandole/Cpp-Handbook/blob/main/AdvancedCppTopics/WeakPointer.md#weak-pointer)
- [5.4.4 Auto Pointer (`std::auto_ptr`) (Deprecated)](#544-auto-pointer-stdauto_ptr-deprecated)

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
