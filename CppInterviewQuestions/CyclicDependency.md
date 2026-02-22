# [Cyclic Dependency](#cyclic-dependency)

## 1️⃣ What Is Cyclic Dependency?

A **cyclic dependency** happens when two (or more) objects **own each other directly or indirectly**, forming a cycle.

When using `std::shared_ptr`, this becomes dangerous because:

* `shared_ptr` uses **reference counting**
* Objects are destroyed only when reference count becomes **0**
* In a cycle → reference count never becomes 0
* Memory leak occurs

---

# 2️⃣ Basic Example of Cyclic Dependency

```cpp
#include <iostream>
#include <memory>

class B;  // forward declaration

class A {
public:
    std::shared_ptr<B> bptr;
    ~A() { std::cout << "A destroyed\n"; }
};

class B {
public:
    std::shared_ptr<A> aptr;
    ~B() { std::cout << "B destroyed\n"; }
};

int main() {
    auto a = std::make_shared<A>();
    auto b = std::make_shared<B>();

    a->bptr = b;
    b->aptr = a;
}
```

---

## What Happens Here?

Step-by-step:

1. `a` created → ref count = 1
2. `b` created → ref count = 1
3. `a->bptr = b` → `b` ref count = 2
4. `b->aptr = a` → `a` ref count = 2

When `main()` ends:

* local `a` destroyed → count becomes 1
* local `b` destroyed → count becomes 1

Both objects still reference each other.

So:

```
A → B
B → A
```

Neither reaches ref count 0.

❌ Destructors never called
❌ Memory leak

---

# 3️⃣ Why Reference Counting Fails Here

`shared_ptr` only counts:

* How many `shared_ptr`s point to object

It does NOT detect:

* Graph cycles
* Logical ownership
* Garbage collection cycles

Reference counting cannot break circular graphs.

---

# 4️⃣ Real-World Example

## Parent-Child Relationship (Wrong Design)

```cpp
class Parent;
class Child;

class Parent {
public:
    std::shared_ptr<Child> child;
};

class Child {
public:
    std::shared_ptr<Parent> parent;  // Problem!
};
```

This creates a cycle.

---

# 5️⃣ Correct Solution → Use `std::weak_ptr`

A `weak_ptr`:

* Does NOT increase reference count
* Does NOT own object
* Only observes object

---

## Fixing the Example

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
    std::weak_ptr<A> aptr;  // changed to weak_ptr
    ~B() { std::cout << "B destroyed\n"; }
};

int main() {
    auto a = std::make_shared<A>();
    auto b = std::make_shared<B>();

    a->bptr = b;
    b->aptr = a;
}
```

---

## What Happens Now?

1. `a` ref count = 1
2. `b` ref count = 1
3. `a->bptr = b` → `b` ref count = 2
4. `b->aptr = a` → NO change in `a` ref count

When main ends:

* `a` destroyed → count 0 → destroyed
* `b` destroyed → count 0 → destroyed

✅ No leak
✅ Destructors called

---

# 6️⃣ Why weak_ptr Solves It

Because:

```
shared_ptr → increases strong count
weak_ptr   → increases weak count only
```

Object lifetime depends ONLY on strong count.

So cycle is broken.

---

# 7️⃣ Visual Diagram

## ❌ Without weak_ptr

```
[A] strong → [B]
[B] strong → [A]

Strong count never zero
```

---

## ✅ With weak_ptr

```
[A] strong → [B]
[B] weak   → [A]

Only one strong direction
```

Now destruction works.

---

# 8️⃣ How To Safely Use weak_ptr

You cannot directly access object:

```cpp
std::weak_ptr<A> wp;
wp->something;  // ❌ error
```

You must use:

```cpp
if (auto sp = wp.lock()) {
    sp->doSomething();
}
```

`lock()`:

* Returns shared_ptr if object alive
* Returns nullptr if destroyed

---

# 9️⃣ Common Real Scenarios

### 1️⃣ Tree Structure

```cpp
class Node {
public:
    std::vector<std::shared_ptr<Node>> children;
    std::weak_ptr<Node> parent;  // weak!
};
```

Children own parent? No.
Parent owns children? Yes.

Correct ownership direction is important.

---

### 2️⃣ Observer Pattern

Observers should NOT extend subject lifetime.

```cpp
class Subject {
    std::vector<std::weak_ptr<Observer>> observers;
};
```

---

### 3️⃣ Graph Structure

Graphs often have cycles.

Use:

* shared_ptr for owning edges
* weak_ptr for back references

---

# 🔟 Advanced Interview Insight

### Q: Can unique_ptr have cyclic dependency?

No.

Because:

* unique_ptr cannot be copied
* Only single owner
* Cycle cannot form via ownership

---

### Q: Does garbage collector solve this?

Languages like Java:

* Use tracing GC
* Can detect cycles

C++:

* Uses deterministic destruction
* No cycle detection
* So weak_ptr required

---

# 1️⃣1️⃣ Detecting Cyclic Leak

You may observe:

* Destructor not called
* Memory usage growing
* Valgrind shows leak

But compiler won't warn you.

---

# 1️⃣2️⃣ Rule of Thumb

Ask:

> Who REALLY owns this object?

If ownership is shared → shared_ptr
If just referencing → weak_ptr

Ownership direction must be clear.

---

# Final Summary

### Cyclic Dependency Problem

* Happens with shared_ptr cycles
* Reference count never reaches zero
* Causes memory leak

### Solution

* Replace one side of cycle with weak_ptr
* weak_ptr does not increase strong count
* Breaks ownership loop

---

# One-Line Interview Answer

> Cyclic dependency occurs when two shared_ptr objects own each other, preventing reference count from reaching zero. It is resolved by replacing one side of the relationship with weak_ptr to break the ownership cycle.

---
