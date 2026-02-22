## [Unique Pointer](#unique-pointer)

`std::unique_ptr` is a smart pointer introduced in **C++11** that represents **exclusive ownership** of a dynamically allocated object.

It ensures:

* Only **one owner** at a time
* Automatic memory cleanup (RAII)
* No accidental copying
* Safer alternative to raw pointers

Defined in:

```cpp
#include <memory>
```

---

# 1️⃣ Basic Idea – Exclusive Ownership

Only one `std::unique_ptr` can own a resource.

```cpp
#include <iostream>
#include <memory>

int main() {
    std::unique_ptr<int> ptr = std::make_unique<int>(10);

    std::cout << *ptr << std::endl;  // 10

} // automatically deletes memory
```

When `ptr` goes out of scope → memory is automatically deleted.

No need for:

```cpp
delete ptr;
```

---

# 2️⃣ Why Not Raw Pointer?

### ❌ Raw Pointer Problem

```cpp
int* ptr = new int(10);
// if exception occurs before delete → memory leak
delete ptr;
```

### ✅ With `unique_ptr`

```cpp
std::unique_ptr<int> ptr = std::make_unique<int>(10);
// automatically cleaned
```

---

# 3️⃣ Copy is NOT Allowed

`unique_ptr` cannot be copied.

```cpp
std::unique_ptr<int> p1 = std::make_unique<int>(10);
std::unique_ptr<int> p2 = p1;   // ❌ ERROR
```

Because two owners are not allowed.

---

# 4️⃣ Move Semantics (Transfer Ownership)

Ownership can be transferred using `std::move`.

```cpp
std::unique_ptr<int> p1 = std::make_unique<int>(10);
std::unique_ptr<int> p2 = std::move(p1);

if (!p1)
    std::cout << "p1 is now null\n";

std::cout << *p2 << std::endl;
```

After move:

* `p1` → nullptr
* `p2` → owns resource

---

# 5️⃣ Internal Implementation (Simplified Version)

This is a simplified custom implementation:

```cpp
template<typename T>
class UniquePtr {
    T* ptr;

public:
    explicit UniquePtr(T* p = nullptr) : ptr(p) {}

    ~UniquePtr() {
        delete ptr;
    }

    // Delete copy constructor
    UniquePtr(const UniquePtr&) = delete;

    // Delete copy assignment
    UniquePtr& operator=(const UniquePtr&) = delete;

    // Move constructor
    UniquePtr(UniquePtr&& other) noexcept {
        ptr = other.ptr;
        other.ptr = nullptr;
    }

    // Move assignment
    UniquePtr& operator=(UniquePtr&& other) noexcept {
        if (this != &other) {
            delete ptr;
            ptr = other.ptr;
            other.ptr = nullptr;
        }
        return *this;
    }

    T& operator*() { return *ptr; }
    T* operator->() { return ptr; }
};
```

This shows:

* Copy deleted
* Move implemented
* Destructor deletes memory

---

# 6️⃣ Creating `unique_ptr`

### ✅ Recommended: `make_unique`

```cpp
auto ptr = std::make_unique<int>(5);
```

Benefits:

* Exception safe
* Cleaner syntax
* No raw `new`

---

### ⚠️ Old Way (Avoid)

```cpp
std::unique_ptr<int> ptr(new int(5));
```

---

# 7️⃣ Custom Deleter

You can provide custom delete logic.

```cpp
void customDeleter(int* ptr) {
    std::cout << "Deleting pointer\n";
    delete ptr;
}

int main() {
    std::unique_ptr<int, decltype(&customDeleter)>
        ptr(new int(10), customDeleter);
}
```

Useful for:

* File handles
* Database connections
* Sockets
* OS resources

Example with `FILE*`:

```cpp
#include <cstdio>
#include <memory>

int main() {
    std::unique_ptr<FILE, decltype(&fclose)>
        file(fopen("test.txt", "w"), &fclose);
}
```

---

# 8️⃣ Unique Pointer with Arrays

```cpp
std::unique_ptr<int[]> arr = std::make_unique<int[]>(5);

arr[0] = 10;
arr[1] = 20;
```

Internally calls `delete[]`.

---

# 9️⃣ `release()`, `reset()`, `get()`

### 🔹 `get()`

Returns raw pointer (without releasing ownership)

```cpp
int* raw = ptr.get();
```

---

### 🔹 `release()`

Releases ownership and returns raw pointer.

```cpp
int* raw = ptr.release();  // ptr becomes null
delete raw;                // you must delete manually
```

---

### 🔹 `reset()`

Deletes current object and optionally takes new one.

```cpp
ptr.reset(new int(20));
```

---

# 🔟 Passing `unique_ptr` to Functions

### ✅ Transfer Ownership

```cpp
void process(std::unique_ptr<int> ptr) {
    std::cout << *ptr << std::endl;
}

process(std::make_unique<int>(5));
```

---

### ✅ Pass Without Transfer (Reference)

```cpp
void process(const std::unique_ptr<int>& ptr) {
    std::cout << *ptr << std::endl;
}
```

---

# 1️⃣1️⃣ Polymorphism with `unique_ptr`

```cpp
class Base {
public:
    virtual void show() {
        std::cout << "Base\n";
    }
    virtual ~Base() = default;
};

class Derived : public Base {
public:
    void show() override {
        std::cout << "Derived\n";
    }
};

int main() {
    std::unique_ptr<Base> ptr = std::make_unique<Derived>();
    ptr->show();
}
```

⚠️ Base destructor must be virtual.

---

# 1️⃣2️⃣ `unique_ptr` vs `shared_ptr`

| Feature      | unique_ptr | shared_ptr         |
| ------------ | ---------- | ------------------ |
| Ownership    | Single     | Multiple           |
| Overhead     | Very low   | Higher (ref count) |
| Copy allowed | ❌          | ✅                  |
| Move allowed | ✅          | ✅                  |
| Performance  | Fastest    | Slower             |

Use `unique_ptr` by default.
Switch to `shared_ptr` only if multiple ownership is required.

---

# 1️⃣3️⃣ When To Use

Use `unique_ptr` when:

* Object has single owner
* Factory pattern
* Resource management
* PIMPL idiom
* Returning large objects from function

---

# 1️⃣4️⃣ Returning `unique_ptr` from Function

```cpp
std::unique_ptr<int> createValue() {
    return std::make_unique<int>(100);
}

int main() {
    auto ptr = createValue();
}
```

Move semantics automatically applied.

---

# 1️⃣5️⃣ Memory Layout

Internally:

```
unique_ptr
  |
  ---> raw pointer (T*)
```

It is almost same size as raw pointer (unless custom deleter stateful).

---

# 1️⃣6️⃣ Important Interview Points

* Copy constructor deleted
* Move constructor defined
* Zero overhead abstraction
* RAII principle
* Custom deleter changes type
* Cannot use with incomplete type destructor unless defined later
* Prefer `make_unique`

---

# 1️⃣7️⃣ Common Mistakes

❌ Forgetting `std::move`

```cpp
p2 = p1;  // ERROR
p2 = std::move(p1); // correct
```

❌ Using after move

```cpp
std::move(p1);
*p1; // undefined behavior
```

❌ Mixing raw delete with unique_ptr

---

# 1️⃣8️⃣ Performance Insight

`unique_ptr` has:

* No reference counting
* No atomic operations
* Almost same performance as raw pointer

That’s why it’s preferred in modern C++.

---

# Final Summary

`std::unique_ptr`:

* Exclusive ownership smart pointer
* Automatically deletes memory
* Non-copyable
* Movable
* Lightweight
* Exception safe
* Supports custom deleters
* Best default smart pointer choice

---
