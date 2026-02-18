# [explicit](#explicit)

`explicit` prevents **implicit conversions** and **implicit constructor calls**.

It is mainly used with:

* Constructors
* Conversion operators (since C++11)
* Deduction guides (since C++17)

It improves **type safety and prevents accidental conversions**.

---

## 🔹 1️⃣ Problem Without `explicit`

Consider this class:

```cpp
#include <iostream>
using namespace std;

class Test {
    int x;
public:
    Test(int val) : x(val) {}
};

void print(Test t) {
    cout << "Called\n";
}

int main() {
    print(10);   // 😱 Implicit conversion!
}
```

#### What happens?

`10` → converted to `Test(10)` automatically.

This is called a **converting constructor**.

Sometimes this is dangerous and unintended.

---

## 🔹 2️⃣ Fix Using `explicit`

```cpp
class Test {
    int x;
public:
    explicit Test(int val) : x(val) {}
};
```

Now:

```cpp
print(10);     // ❌ Error
print(Test(10)); // ✅ OK
```

Now conversion must be intentional.

---

## 🔹 3️⃣ What Exactly Does `explicit` Do?

It prevents:

* Implicit conversion in function calls
* Implicit conversion in copy initialization
* Accidental temporary object creation

It forces:

* Direct initialization

---

## 🔹 4️⃣ Direct vs Copy Initialization

#### Without explicit

```cpp
Test t1 = 10;  // implicit conversion (copy initialization)
Test t2(10);   // direct initialization
```

#### With explicit

```cpp
Test t1 = 10;  // ❌ Error
Test t2(10);   // ✅ OK
```

---

## 🔹 5️⃣ explicit with Multiple Parameters?

Before C++11:
Only single-argument constructors were conversion constructors.

After C++11:
Any constructor that can be called with single argument is a converting constructor.

Example:

```cpp
class A {
public:
    A(int x, int y = 0) {}
};
```

This can act as converting constructor.

So use:

```cpp
explicit A(int x, int y = 0) {}
```

---

## 🔹 6️⃣ explicit with Conversion Operators (C++11)

```cpp
class Test {
public:
    explicit operator int() const {
        return 42;
    }
};
```

Without explicit:

```cpp
Test t;
int x = t;   // implicit conversion
```

With explicit:

```cpp
int x = static_cast<int>(t);  // must cast explicitly
```

---

## 🔹 7️⃣ explicit(bool) in C++20

You can make explicit conditional.

```cpp
template<typename T>
class Wrapper {
public:
    explicit(!std::is_integral_v<T>)
    Wrapper(T val) {}
};
```

If condition true → constructor is explicit.

---

## 🔹 8️⃣ Real-World Example (Why It Matters)

Imagine this:

```cpp
class Distance {
    int meters;
public:
    Distance(int m) : meters(m) {}
};

void travel(Distance d);
```

Now:

```cpp
travel(5); // 5 meters? 5 km? dangerous
```

Better:

```cpp
explicit Distance(int m);
```

Forces clarity:

```cpp
travel(Distance(5));
```

---

## 🔹 9️⃣ explicit and STL

Many STL constructors are marked explicit to prevent unsafe conversions.

Example:

```cpp
std::vector<int> v = 5;  // ❌
std::vector<int> v(5);   // ✅ size 5
```

---

## 🔹 🔟 explicit vs delete

Another way to prevent conversion:

```cpp
class A {
public:
    A(int) = delete;
};
```

But that completely disallows it.

`explicit` allows direct use but blocks implicit conversion.

---

## 🔹 1️⃣1️⃣ Interview Tricky Case

```cpp
class A {
public:
    explicit A(int x) {}
};

A a1(10);   // OK
A a2 = 10;  // Error
A a3 = A(10); // OK
```

Why last works?
Because explicit only blocks implicit conversion, not direct construction.

---

## 🔹 1️⃣2️⃣ When Should You Use explicit?

✔ Single-argument constructors
✔ Constructors with default arguments
✔ Conversion operators
✔ Wrapper types
✔ Value classes
✔ Strongly typed APIs

---

## 🔹 1️⃣3️⃣ When NOT to Use explicit?

When implicit conversion is:

* Safe
* Natural
* Intended

Example:
`std::string` from `"hello"`

```cpp
std::string s = "hello"; // allowed
```

---

## 🔹 1️⃣4️⃣ explicit and Inheritance

Explicit constructors are not inherited unless specified.

```cpp
class Base {
public:
    explicit Base(int) {}
};

class Derived : public Base {
    using Base::Base;
};
```

Derived constructor remains explicit.

---

## 🔹 1️⃣5️⃣ explicit + Aggregate Initialization

`explicit` does not affect brace initialization directly:

```cpp
A a{10};  // OK
```

But affects copy-list initialization:

```cpp
A a = {10}; // ❌
```

---

## 🔹 1️⃣6️⃣ Summary Table

| Without explicit            | With explicit |
| --------------------------- | ------------- |
| Implicit conversion allowed | Not allowed   |
| Risk of bugs                | Safer         |
| Less control                | More control  |
| Cleaner syntax              | More strict   |

---

## 🔥 Ultimate Summary

`explicit` is about:

* Preventing accidental implicit conversions
* Making APIs safer
* Improving type clarity
* Avoiding hidden temporary object creation

It is one of the most important keywords for:

* Library design
* Large-scale systems
* Interview coding questions

---

## 🔥 Senior-Level Insigh

Modern C++ best practice:

👉 Mark all single-argument constructors `explicit` by default
👉 Remove `explicit` only if implicit conversion is clearly desired

---
