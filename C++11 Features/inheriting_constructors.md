# [Inheriting Constructors](#inheriting-constructors)


## 🔹 1️⃣ What Problem Did It Solve?

Before C++11, if a base class had multiple constructors, the derived class had to manually forward them.

Example (Pre-C++11):

```cpp
class Base {
public:
    Base(int x) {}
    Base(int x, double y) {}
};

class Derived : public Base {
public:
    Derived(int x) : Base(x) {}
    Derived(int x, double y) : Base(x, y) {}
};
```

Problem:

* Repetitive
* Error-prone
* Hard to maintain if base adds new constructors

---

## 🔹 2️⃣ What Are Inheriting Constructors?

C++11 allows a derived class to inherit constructors from its base class using:

```cpp
using Base::Base;
```

---

## 🔹 3️⃣ Basic Example

```cpp
#include <iostream>

class Base {
public:
    Base(int x) {
        std::cout << "Base(int)\n";
    }

    Base(int x, double y) {
        std::cout << "Base(int, double)\n";
    }
};

class Derived : public Base {
public:
    using Base::Base;   // inherit all constructors
};
```

Now:

```cpp
int main() {
    Derived d1(10);
    Derived d2(10, 3.14);
}
```

Output:

```
Base(int)
Base(int, double)
```

No need to manually define forwarding constructors.

---

## 🔹 4️⃣ What Actually Happens Internally?

`using Base::Base;` tells compiler:

> “Make Base’s constructors visible as constructors of Derived.”

Compiler generates forwarding constructors automatically.

Equivalent to writing:

```cpp
Derived(int x) : Base(x) {}
Derived(int x, double y) : Base(x, y) {}
```

---

## 🔹 5️⃣ Important Rule: Base Initialization Still Happens

Even with inherited constructors:

* Base part is initialized first
* Then Derived part

Standard object construction rules still apply.

---

## 🔹 6️⃣ Derived Members Are Still Default-Initialized

Example:

```cpp
class Derived : public Base {
    int z;

public:
    using Base::Base;
};
```

If you do:

```cpp
Derived d(10);
```

* Base initialized with 10
* `z` default-initialized (uninitialized if built-in)

Important interview point.

---

## 🔹 7️⃣ Overriding Specific Constructors

You can still define your own constructor:

```cpp
class Derived : public Base {
public:
    using Base::Base;

    Derived(int x) : Base(x) {
        std::cout << "Custom Derived constructor\n";
    }
};
```

Now this overrides inherited one for that signature.

---

## 🔹 8️⃣ Access Control Matters

If base constructor is private:

```cpp
class Base {
private:
    Base(int x) {}
};

class Derived : public Base {
public:
    using Base::Base;   // ❌ error (not accessible)
};
```

Inheriting constructors does NOT bypass access control.

---

## 🔹 9️⃣ Works with Protected Constructors

```cpp
class Base {
protected:
    Base(int x) {}
};

class Derived : public Base {
public:
    using Base::Base;   // OK
};
```

---

## 🔹 🔟 Multiple Inheritance Case

If two base classes have same constructor signature:

```cpp
class A {
public:
    A(int) {}
};

class B {
public:
    B(int) {}
};

class C : public A, public B {
public:
    using A::A;
    using B::B;
};
```

Calling:

```cpp
C c(5);  // ❌ ambiguous
```

Compiler error due to ambiguity.

---

## 🔹 1️⃣1️⃣ Why It’s Useful

✔ Reduce boilerplate
✔ Cleaner derived classes
✔ Useful in wrapper classes
✔ Common in CRTP and mixins
✔ Maintains DRY principle

---

## 🔹 1️⃣2️⃣ Real-World Example

```cpp
class ExceptionBase {
public:
    ExceptionBase(const std::string& msg) {}
};

class FileException : public ExceptionBase {
public:
    using ExceptionBase::ExceptionBase;
};
```

No need to rewrite constructor.

---

## 🔹 1️⃣3️⃣ Important Limitation

Inheriting constructors:

* Do NOT inherit default constructor if base doesn’t have one
* Do NOT inherit copy/move constructors automatically
* Do NOT inherit assignment operators

---

## 🔹 1️⃣4️⃣ Difference from Delegating Constructors

| Feature                  | Delegating         | Inheriting         |
| ------------------------ | ------------------ | ------------------ |
| Works within same class  | ✅                  | ❌                  |
| Works across inheritance | ❌                  | ✅                  |
| Syntax                   | `: ClassName(...)` | `using Base::Base` |

---

## 🔹 1️⃣5️⃣ Interview-Level Explanation

If interviewer asks:

> What are inheriting constructors?

Answer:

> Inheriting constructors, introduced in C++11, allow a derived class to reuse constructors of its base class using `using Base::Base;`. This avoids manually writing forwarding constructors and reduces boilerplate. Access control rules still apply, and derived members are initialized normally after the base constructor executes.

---

## 🔹 1️⃣6️⃣ Key Takeaways

✔ Introduced in C++11
✔ Use `using Base::Base;`
✔ Reduces boilerplate
✔ Does not override access control
✔ Base constructed first

---
