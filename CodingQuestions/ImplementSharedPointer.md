
# [Custom Implementation of Shared Pointer](#custom-implementation-of-shared-pointer)

```cpp
#include <iostream>
#include <utility>   // std::move, std::swap
#include <cassert>

template <typename T>
class SharedPtr {
private:
    T* ptr;              // Managed object
    size_t* refCount;    // Shared reference counter

    // Helper function to decrease ref count and cleanup
    void release() noexcept {
        if (refCount) {
            (*refCount)--;

            if (*refCount == 0) {
                delete ptr;        // Delete managed object
                delete refCount;   // Delete counter
                std::cout << "Resource deleted\n";
            }
        }
    }

public:
    // 1️⃣ Default constructor
    SharedPtr() noexcept : ptr(nullptr), refCount(nullptr) {}

    // 2️⃣ Constructor from raw pointer
    explicit SharedPtr(T* p) : ptr(p) {
        if (p) {
            refCount = new size_t(1);
        } else {
            refCount = nullptr;
        }
    }

    // 3️⃣ Destructor
    ~SharedPtr() {
        release();
    }

    // 4️⃣ Copy constructor
    SharedPtr(const SharedPtr& other) noexcept
        : ptr(other.ptr), refCount(other.refCount) {

        if (refCount) {
            (*refCount)++;
        }
    }

    // 5️⃣ Copy assignment
    SharedPtr& operator=(const SharedPtr& other) noexcept {
        if (this != &other) {
            release();

            ptr = other.ptr;
            refCount = other.refCount;

            if (refCount) {
                (*refCount)++;
            }
        }
        return *this;
    }

    // 6️⃣ Move constructor
    SharedPtr(SharedPtr&& other) noexcept
        : ptr(other.ptr), refCount(other.refCount) {

        other.ptr = nullptr;
        other.refCount = nullptr;
    }

    // 7️⃣ Move assignment
    SharedPtr& operator=(SharedPtr&& other) noexcept {
        if (this != &other) {
            release();

            ptr = other.ptr;
            refCount = other.refCount;

            other.ptr = nullptr;
            other.refCount = nullptr;
        }
        return *this;
    }

    // 8️⃣ Dereference operator
    T& operator*() const {
        assert(ptr != nullptr && "Dereferencing null pointer!");
        return *ptr;
    }

    // 9️⃣ Arrow operator
    T* operator->() const noexcept {
        return ptr;
    }

    // 🔟 get()
    T* get() const noexcept {
        return ptr;
    }

    // 1️⃣1️⃣ use_count()
    size_t use_count() const noexcept {
        return refCount ? *refCount : 0;
    }

    // 1️⃣2️⃣ reset()
    void reset(T* p = nullptr) {
        release();

        if (p) {
            ptr = p;
            refCount = new size_t(1);
        } else {
            ptr = nullptr;
            refCount = nullptr;
        }
    }

    // 1️⃣3️⃣ swap()
    void swap(SharedPtr& other) noexcept {
        std::swap(ptr, other.ptr);
        std::swap(refCount, other.refCount);
    }

    // 1️⃣4️⃣ bool conversion
    explicit operator bool() const noexcept {
        return ptr != nullptr;
    }
};
```

---

# 🔹 Test Class for Demonstration

```cpp
class Test {
public:
    int value;

    Test(int v) : value(v) {
        std::cout << "Constructor called for " << value << "\n";
    }

    ~Test() {
        std::cout << "Destructor called for " << value << "\n";
    }

    void show() {
        std::cout << "Value: " << value << "\n";
    }
};
```

---

# 🔹 main() – Simulation & Validation

```cpp
int main() {

    // 1️⃣ Basic ownership
    SharedPtr<Test> p1(new Test(10));
    std::cout << "p1 count: " << p1.use_count() << "\n";

    // 2️⃣ Copy constructor
    SharedPtr<Test> p2 = p1;
    std::cout << "After copy:\n";
    std::cout << "p1 count: " << p1.use_count() << "\n";
    std::cout << "p2 count: " << p2.use_count() << "\n";

    // 3️⃣ Move constructor
    SharedPtr<Test> p3 = std::move(p2);
    std::cout << "After move:\n";
    std::cout << "p2 count: " << p2.use_count() << "\n";
    std::cout << "p3 count: " << p3.use_count() << "\n";

    // 4️⃣ Reset
    p3.reset(new Test(20));
    std::cout << "After reset p3:\n";
    std::cout << "p3 count: " << p3.use_count() << "\n";

    // 5️⃣ Swap
    SharedPtr<Test> p4(new Test(30));
    SharedPtr<Test> p5(new Test(40));

    p4.swap(p5);

    std::cout << "After swap:\n";
    p4->show();  // Should show 40
    p5->show();  // Should show 30

    // 6️⃣ Automatic destruction when last reference goes out of scope
    return 0;
}
```

---

# 🔹 Expected Behavior (Conceptually)

```
Constructor called for 10
p1 count: 1
After copy:
p1 count: 2
p2 count: 2
After move:
p2 count: 0
p3 count: 2
Constructor called for 20
Destructor called for 10
After reset p3:
p3 count: 1
Constructor called for 30
Constructor called for 40
After swap:
Value: 40
Value: 30
Destructor called for 20
Destructor called for 30
Destructor called for 40
Resource deleted
```

---
