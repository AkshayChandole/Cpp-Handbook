# [Object Slicing](#object-slicing)

## 🔹 1️⃣ What Is Object Slicing?

Object slicing happens when:

> A derived class object is assigned to a base class object **by value**.

The derived-specific part gets “sliced off”.

---

## 🔹 2️⃣ Simple Example

```cpp
#include <iostream>

class Base {
public:
    int x;

    Base(int val) : x(val) {}

    virtual void show() const {
        std::cout << "Base x = " << x << "\n";
    }
};

class Derived : public Base {
public:
    int y;

    Derived(int a, int b) : Base(a), y(b) {}

    void show() const override {
        std::cout << "Derived x = " << x
                  << ", y = " << y << "\n";
    }
};

int main() {

    Derived d(10, 20);

    Base b = d;   // ⚠️ Object slicing happens here

    b.show();     // What will this print?
}
```

---

## 🔹 Output

```
Base x = 10
```

NOT:

```
Derived x = 10, y = 20
```

Why?

Because `y` was sliced away.

---

## 🔹 3️⃣ What Happens Internally (Memory Layout)

Let’s see memory layout.

### Derived object in memory:

```
+------------------+
| Base::x          |
| vptr             |
+------------------+
| Derived::y       |
+------------------+
```

When we do:

```cpp
Base b = d;
```

The compiler:

* Copies only the **Base portion**
* Ignores Derived part

Resulting `b`:

```
+------------------+
| Base::x          |
| vptr             |
+------------------+
```

The `y` member is gone.

That is slicing.

---

## 🔹 4️⃣ Why Virtual Doesn’t Save You

Even though `show()` is virtual, slicing still happens.

Why?

Because after slicing:

```cpp
Base b = d;
```

`b` is a completely separate Base object.

Its dynamic type is:

```
Base
```

Not Derived.

So virtual dispatch calls:

```
Base::show()
```

---

## 🔹 5️⃣ When Slicing Happens

Slicing occurs when:

✔ Passing by value
✔ Returning base type by value
✔ Storing derived in base container by value

Example:

```cpp
void print(Base b) {   // ⚠️ pass by value
    b.show();
}
```

Calling:

```cpp
Derived d(1, 2);
print(d);
```

→ slicing happens.

---

## 🔹 6️⃣ How To Prevent Object Slicing

#### ✅ Use references

```cpp
void print(const Base& b) {
    b.show();
}
```

Now:

* No copy
* No slicing
* Virtual dispatch works

---

#### ✅ Use pointers

```cpp
void print(const Base* b) {
    b->show();
}
```

---

#### ✅ Use smart pointers

```cpp
std::shared_ptr<Base> ptr = std::make_shared<Derived>(1, 2);
```

No slicing.

---

## 🔹 7️⃣ Dangerous Real-World Example

```cpp
std::vector<Base> vec;

vec.push_back(Derived(1, 2));  // slicing!
```

Each element becomes Base.

Solution:

```cpp
std::vector<std::unique_ptr<Base>> vec;
```

---

## 🔹 8️⃣ Advanced Example (Constructor Order)

```cpp
Derived d(10, 20);
Base b = d;
```

Sequence:

1. Derived constructed
2. Base copy constructor called
3. Only Base part copied
4. Derived part discarded

No Derived copy constructor involved.

---

## 🔹 9️⃣ Why C++ Allows Slicing

Because C++:

* Supports value semantics
* Doesn’t enforce polymorphism
* Treats Base and Derived as distinct types

This is design choice for performance and flexibility.

---

## 🔹 🔥 Important Concept: Static vs Dynamic Type

| Concept      | Meaning                       |
| ------------ | ----------------------------- |
| Static type  | Type known at compile time    |
| Dynamic type | Actual object type at runtime |

After slicing:

```cpp
Base b = d;
```

Static type = Base
Dynamic type = Base

Because it’s a separate Base object.

---

## 🔹 1️⃣0️⃣ Visual Summary

Before slicing:

```
Derived object
  |
  ├─ Base part
  └─ Derived part
```

After slicing:

```
Base object only
```

Derived portion is gone permanently.

---

## 🔹 1️⃣1️⃣ Interview-Level Explanation

If interviewer asks:

> Explain object slicing.

Answer:

> Object slicing occurs when a derived object is assigned to a base object by value, causing the derived-specific members to be discarded. Only the base portion is copied, and the resulting object becomes a pure base object. This breaks polymorphism because virtual dispatch depends on dynamic type, which after slicing becomes the base type. Slicing can be avoided by passing or storing objects via references or pointers.

---

## 🔹 1️⃣2️⃣ Advanced Discussion 

Slicing can also break:

* Invariants
* Resource ownership
* State consistency

If derived class manages extra resources, slicing can silently drop them.

---

## 🔹 Final Takeaway

Object slicing:

✔ Happens on copy by value
✔ Removes derived portion
✔ Breaks polymorphism
✔ Avoided using references/pointers

---
