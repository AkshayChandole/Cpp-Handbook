# [Shared Pointer](#shared-pointer)

`std::shared_ptr` is a smart pointer that provides **shared ownership** of a dynamically allocated object.

Multiple `shared_ptr`s can point to the same object.
The object is destroyed **only when the last shared_ptr releases it**.

Defined in:

```cpp
#include <memory>
```

---

# 1️⃣ Basic Idea – Reference Counting

```cpp
#include <iostream>
#include <memory>

int main() {
    std::shared_ptr<int> p1 = std::make_shared<int>(10);
    std::shared_ptr<int> p2 = p1;  // shared ownership

    std::cout << p1.use_count() << std::endl; // 2
}
```

When:

* `p1` destroyed → count becomes 1
* `p2` destroyed → count becomes 0 → memory deleted

---

# 2️⃣ How It Works Internally

`shared_ptr` maintains:

```
shared_ptr
   |
   +----> Managed Object (T*)
   |
   +----> Control Block
            - reference count
            - weak count
            - custom deleter
            - allocator info
```

When reference count becomes zero → object destroyed
When weak count also zero → control block destroyed

---

# 3️⃣ Creating `shared_ptr`

### ✅ Recommended: `make_shared`

```cpp
auto ptr = std::make_shared<int>(5);
```

Why preferred?

* Single memory allocation (object + control block)
* Better performance
* Exception safe

---

### ⚠️ Not Recommended (but valid)

```cpp
std::shared_ptr<int> ptr(new int(5));
```

Two allocations:

* Object
* Control block

Less efficient.

---

# 4️⃣ Reference Count Behavior

```cpp
auto p1 = std::make_shared<int>(100);
std::cout << p1.use_count();  // 1

auto p2 = p1;
std::cout << p1.use_count();  // 2

{
    auto p3 = p2;
    std::cout << p1.use_count();  // 3
}

std::cout << p1.use_count();  // 2
```

Count increases on:

* Copy construction
* Copy assignment

Count decreases on:

* Destruction
* Reset

---

# 5️⃣ Custom Implementation (Simplified)

```cpp
template<typename T>
class SharedPtr {
    T* ptr;
    size_t* refCount;

public:
    explicit SharedPtr(T* p = nullptr)
        : ptr(p), refCount(new size_t(1)) {}

    ~SharedPtr() {
        (*refCount)--;
        if (*refCount == 0) {
            delete ptr;
            delete refCount;
        }
    }

    // Copy constructor
    SharedPtr(const SharedPtr& other) {
        ptr = other.ptr;
        refCount = other.refCount;
        (*refCount)++;
    }

    // Copy assignment
    SharedPtr& operator=(const SharedPtr& other) {
        if (this != &other) {
            (*refCount)--;
            if (*refCount == 0) {
                delete ptr;
                delete refCount;
            }

            ptr = other.ptr;
            refCount = other.refCount;
            (*refCount)++;
        }
        return *this;
    }

    T& operator*() { return *ptr; }
};
```

Real implementation is more complex (atomic counters, weak count, etc.)

---

# 6️⃣ `shared_ptr` Is Copyable

```cpp
auto p1 = std::make_shared<int>(10);
auto p2 = p1;     // OK
auto p3 = p2;     // OK
```

Unlike `unique_ptr`, copying is allowed.

---

# 7️⃣ Move Semantics

```cpp
auto p1 = std::make_shared<int>(5);
auto p2 = std::move(p1);
```

After move:

* `p1` becomes null
* ref count unchanged

---

# 8️⃣ Reset & Get

### 🔹 reset()

```cpp
p1.reset();  // decreases ref count
```

Or:

```cpp
p1.reset(new int(50));
```

---

### 🔹 get()

```cpp
int* raw = p1.get();
```

⚠️ Do NOT delete raw pointer manually.

---

# 9️⃣ Custom Deleter

```cpp
void deleter(int* ptr) {
    std::cout << "Deleting\n";
    delete ptr;
}

std::shared_ptr<int> ptr(new int(10), deleter);
```

Custom deleter is stored in control block.

---

# 🔟 Shared Pointer with Polymorphism

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
    std::shared_ptr<Base> ptr = std::make_shared<Derived>();
    ptr->show();
}
```

Base destructor must be virtual.

---

# 1️⃣1️⃣ Weak Pointer (VERY IMPORTANT)

### Problem: Circular Reference

```cpp
class B;

class A {
public:
    std::shared_ptr<B> bptr;
};

class B {
public:
    std::shared_ptr<A> aptr;
};
```

If both point to each other → reference count never reaches 0 → memory leak.

---

### Solution: `std::weak_ptr`

```cpp
class B;

class A {
public:
    std::shared_ptr<B> bptr;
};

class B {
public:
    std::weak_ptr<A> aptr;  // no ownership
};
```

`weak_ptr`:

* Does not increase reference count
* Breaks circular dependency

---

# 1️⃣2️⃣ Thread Safety

* Reference count is atomic
* Multiple threads can copy shared_ptr safely
* But object itself is NOT thread safe automatically

---

# 1️⃣3️⃣ shared_ptr vs unique_ptr

| Feature         | shared_ptr | unique_ptr |
| --------------- | ---------- | ---------- |
| Ownership       | Multiple   | Single     |
| Copyable        | ✅          | ❌          |
| Moveable        | ✅          | ✅          |
| Memory Overhead | High       | Low        |
| Performance     | Slower     | Faster     |
| Control Block   | Yes        | No         |

---

# 1️⃣4️⃣ Memory Overhead

`shared_ptr` contains:

* Pointer to object
* Pointer to control block

Control block contains:

* strong count
* weak count
* deleter
* allocator

Hence larger and slower than `unique_ptr`.

---

# 1️⃣5️⃣ Passing shared_ptr to Functions

### By Value (Increase count)

```cpp
void process(std::shared_ptr<int> ptr) {
    std::cout << ptr.use_count();
}
```

### By Reference (No count change)

```cpp
void process(const std::shared_ptr<int>& ptr) {
    std::cout << ptr.use_count();
}
```

Prefer reference unless ownership needed.

---

# 1️⃣6️⃣ Aliasing Constructor (Advanced)

```cpp
auto p = std::make_shared<std::vector<int>>(10);

std::shared_ptr<int> alias(p, &(*p)[0]);
```

* Shares same control block
* Points to different address

---

# 1️⃣7️⃣ enable_shared_from_this (Very Important)

If object wants to create shared_ptr of itself:

```cpp
#include <memory>

class MyClass : public std::enable_shared_from_this<MyClass> {
public:
    std::shared_ptr<MyClass> getPtr() {
        return shared_from_this();
    }
};
```

Without this → creating shared_ptr(this) causes double deletion.

---

# 1️⃣8️⃣ Performance Insight

`shared_ptr` overhead:

* Atomic ref counting
* Control block allocation
* Cache misses

Use only when shared ownership is required.

Default choice should still be:

> `unique_ptr`

---

# 1️⃣9️⃣ Common Mistakes

❌ Creating multiple shared_ptr from same raw pointer

```cpp
int* raw = new int(10);
std::shared_ptr<int> p1(raw);
std::shared_ptr<int> p2(raw); // CRASH (double delete)
```

Always share from existing shared_ptr:

```cpp
auto p1 = std::make_shared<int>(10);
auto p2 = p1;
```

---

❌ Circular reference without weak_ptr
❌ Passing shared_ptr everywhere unnecessarily

---

# 2️⃣0️⃣ When To Use shared_ptr

Use when:

* Multiple objects share resource
* Graph structures
* Observer pattern
* Cache systems
* Plugin systems

Avoid when:

* Clear single ownership
* Performance critical code

---

# Final Summary

`std::shared_ptr`:

* Shared ownership smart pointer
* Uses reference counting
* Thread-safe reference count
* Copyable
* Has control block
* Supports weak_ptr
* More expensive than unique_ptr
* Should be used only when needed

---
