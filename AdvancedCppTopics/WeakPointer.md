# [Weak Pointer](#weak-pointer)

`std::weak_ptr` is a smart pointer that:

* Refers to an object managed by `std::shared_ptr`
* **Does NOT own** the object
* Does NOT increase reference count
* Used mainly to prevent circular references

Defined in:

```cpp
#include <memory>
```

---

# 1️⃣ Why Do We Need `weak_ptr`?

### The Circular Reference Problem

Consider:

```cpp
#include <memory>

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

If:

```cpp
auto a = std::make_shared<A>();
auto b = std::make_shared<B>();

a->bptr = b;
b->aptr = a;
```

Now:

* `a` holds `b`
* `b` holds `a`
* Reference count never becomes 0
* Memory leak ❌

Even when `a` and `b` go out of scope!

---

# 2️⃣ Solution → Use `weak_ptr`

```cpp
#include <memory>

class B;

class A {
public:
    std::shared_ptr<B> bptr;
};

class B {
public:
    std::weak_ptr<A> aptr;  // weak reference
};
```

Now:

* `weak_ptr` does NOT increase reference count
* Circular reference broken
* Memory released correctly

---

# 3️⃣ How `weak_ptr` Works Internally

Recall `shared_ptr` structure:

```
Control Block:
  - strong count
  - weak count
  - deleter
```

When you create:

```cpp
std::weak_ptr<int> wp = sp;
```

* strong count → unchanged
* weak count → increases

Object destroyed when:

* strong count = 0

Control block destroyed when:

* strong count = 0
* weak count = 0

---

# 4️⃣ Basic Example

```cpp
#include <iostream>
#include <memory>

int main() {
    std::shared_ptr<int> sp = std::make_shared<int>(10);
    std::weak_ptr<int> wp = sp;

    std::cout << "use_count: " << sp.use_count() << "\n"; // 1

    if (!wp.expired()) {
        auto temp = wp.lock();
        std::cout << *temp << "\n";
    }
}
```

---

# 5️⃣ Important Member Functions

## 🔹 `lock()`

Converts weak_ptr → shared_ptr safely.

```cpp
std::shared_ptr<int> temp = wp.lock();
```

If object exists → returns shared_ptr
If destroyed → returns empty shared_ptr

---

## 🔹 `expired()`

Checks if object is already destroyed.

```cpp
if (wp.expired()) {
    std::cout << "Object destroyed\n";
}
```

---

## 🔹 `use_count()`

Returns strong reference count.

```cpp
wp.use_count();
```

---

## 🔹 `reset()`

```cpp
wp.reset();
```

Removes weak reference.

---

# 6️⃣ What Happens When Object Is Destroyed?

```cpp
std::weak_ptr<int> wp;

{
    auto sp = std::make_shared<int>(100);
    wp = sp;
}

if (wp.expired())
    std::cout << "Object gone\n";
```

After scope:

* strong count = 0
* object destroyed
* weak_ptr still exists
* `wp.lock()` returns nullptr

---

# 7️⃣ Real-World Use Case: Observer Pattern

Observer should not own subject.

```cpp
class Observer;

class Subject {
public:
    std::vector<std::weak_ptr<Observer>> observers;
};
```

Why weak?

* Subject should not extend observer lifetime
* Avoid circular ownership

---

# 8️⃣ Difference Between shared_ptr and weak_ptr

| Feature                    | shared_ptr | weak_ptr |
| -------------------------- | ---------- | -------- |
| Ownership                  | Yes        | No       |
| Increases ref count        | Yes        | No       |
| Can access object directly | Yes        | No       |
| Needs lock()               | No         | Yes      |
| Prevent circular ref       | No         | Yes      |

---

# 9️⃣ Weak Pointer Cannot Access Object Directly

❌ This is invalid:

```cpp
std::weak_ptr<int> wp;
*wp;       // ERROR
wp->something; // ERROR
```

You must use:

```cpp
if (auto sp = wp.lock()) {
    std::cout << *sp;
}
```

---

# 🔟 Interview-Level Deep Insight

### Why lock()?

Because object might be destroyed between:

```cpp
if (!wp.expired()) {
   // object could die here in multithreaded environment
}
```

So `lock()` is atomic and safe.

---

# 1️⃣1️⃣ Multithreading Insight

`shared_ptr` reference counting is atomic.
`weak_ptr::lock()` is thread safe.

Common pattern:

```cpp
std::shared_ptr<T> temp = wp.lock();
if (temp) {
   // safe usage
}
```

---

# 1️⃣2️⃣ Memory Layout Overview

```
shared_ptr ----\
                 \
                  > Control Block (strong + weak count)
                 /
weak_ptr -------/
```

weak_ptr shares control block
But does NOT own object

---

# 1️⃣3️⃣ Common Mistakes

❌ Using weak_ptr without checking lock

```cpp
auto sp = wp.lock();
std::cout << *sp; // crash if null
```

Always check:

```cpp
if (auto sp = wp.lock()) {
   // safe
}
```

---

❌ Thinking weak_ptr prevents destruction
It does NOT.

---

# 1️⃣4️⃣ Complete Example With Circular Fix

```cpp
#include <iostream>
#include <memory>

class B;

class A {
public:
    std::shared_ptr<B> bptr;
    ~A() { std::cout << "A destroyed\n"; }
};

class B {
public:
    std::weak_ptr<A> aptr;
    ~B() { std::cout << "B destroyed\n"; }
};

int main() {
    auto a = std::make_shared<A>();
    auto b = std::make_shared<B>();

    a->bptr = b;
    b->aptr = a;
}
```

Output:

```
A destroyed
B destroyed
```

Memory released correctly ✅

---

# 1️⃣5️⃣ When To Use weak_ptr

Use when:

* Breaking circular references
* Observer pattern
* Cache systems
* Temporary non-owning references
* Graph structures
* Parent pointer in tree structure

Example: Tree node

```cpp
class Node {
public:
    std::vector<std::shared_ptr<Node>> children;
    std::weak_ptr<Node> parent;
};
```

---

# 1️⃣6️⃣ Performance Insight

weak_ptr:

* Slight overhead
* Shares control block
* No atomic increment for strong count
* lock() does atomic check

Very lightweight compared to shared_ptr.

---

# Final Summary

`std::weak_ptr`:

* Non-owning smart pointer
* Works with shared_ptr
* Prevents circular reference
* Does NOT increase ref count
* Must use lock() to access object
* Used in observer, graph, tree structures

---
