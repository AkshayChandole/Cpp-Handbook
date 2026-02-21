
# [Custom Implementation of Unique Pointer](#custom-implementation-of-unique-pointer)

```cpp
#include <iostream>
#include <utility>   // for std::move, std::swap
#include <cassert>

template <typename T>
class UniquePtr {
private:
    T* ptr;

public:
    // 1️⃣ Default constructor
    UniquePtr() noexcept : ptr(nullptr) {}

    // 2️⃣ Constructor from raw pointer
    explicit UniquePtr(T* p) noexcept : ptr(p) {}

    // 3️⃣ Destructor
    ~UniquePtr() {
        delete ptr;
    }

    // 4️⃣ Delete copy constructor (No copy allowed)
    UniquePtr(const UniquePtr&) = delete;

    // 5️⃣ Delete copy assignment
    UniquePtr& operator=(const UniquePtr&) = delete;

    // 6️⃣ Move constructor
    UniquePtr(UniquePtr&& other) noexcept : ptr(other.ptr) {
        other.ptr = nullptr;
    }

    // 7️⃣ Move assignment
    UniquePtr& operator=(UniquePtr&& other) noexcept {
        if (this != &other) {
            delete ptr;             // Clean existing resource
            ptr = other.ptr;        // Transfer ownership
            other.ptr = nullptr;    // Release source
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

    // 🔟 get() - return raw pointer
    T* get() const noexcept {
        return ptr;
    }

    // 1️⃣1️⃣ release() - release ownership without deleting
    T* release() noexcept {
        T* temp = ptr;
        ptr = nullptr;
        return temp;
    }

    // 1️⃣2️⃣ reset() - replace managed object
    void reset(T* p = nullptr) noexcept {
        delete ptr;
        ptr = p;
    }

    // 1️⃣3️⃣ swap()
    void swap(UniquePtr& other) noexcept {
        std::swap(ptr, other.ptr);
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

# 🔹 `main()` – Simulation & Validation

```cpp
int main() {

    // 1️⃣ Basic ownership
    UniquePtr<Test> p1(new Test(10));
    p1->show();

    // 2️⃣ Move constructor
    UniquePtr<Test> p2 = std::move(p1);

    if (!p1) {
        std::cout << "p1 is now empty after move\n";
    }

    p2->show();

    // 3️⃣ Move assignment
    UniquePtr<Test> p3;
    p3 = std::move(p2);

    if (!p2) {
        std::cout << "p2 is now empty after move assignment\n";
    }

    p3->show();

    // 4️⃣ Reset
    p3.reset(new Test(20));

    // 5️⃣ Release
    Test* raw = p3.release();
    if (!p3) {
        std::cout << "p3 released ownership\n";
    }

    delete raw; // manual delete since ownership released

    // 6️⃣ Swap
    UniquePtr<Test> p4(new Test(30));
    UniquePtr<Test> p5(new Test(40));

    p4.swap(p5);

    p4->show();  // Should show 40
    p5->show();  // Should show 30

    return 0;
}
```

---

# 🔹 Expected Output (Order Matters)

```
Constructor called for 10
Value: 10
p1 is now empty after move
Value: 10
p2 is now empty after move assignment
Value: 10
Constructor called for 20
Destructor called for 10
p3 released ownership
Destructor called for 20
Constructor called for 30
Constructor called for 40
Value: 40
Value: 30
Destructor called for 30
Destructor called for 40
```

---

### ✅ Why copy is deleted?

To enforce **single ownership** semantics.

### ✅ Why move is `noexcept`?

Containers like `std::vector` require noexcept move for optimization.

---

###  How `make_unique` Would Look

```cpp
template <typename T, typename... Args>
UniquePtr<T> make_unique_custom(Args&&... args) {
    return UniquePtr<T>(new T(std::forward<Args>(args)...));
}
```

---
