# [static_assert](#static_assert)

## 🔹 1️⃣ What is `static_assert`?

`static_assert` performs a **compile-time assertion**.

If the condition is false → compilation fails.

Syntax (C++11):

```cpp
static_assert(constant_expression, "error message");
```

* `constant_expression` must be known at compile time.
* Error message is required in C++11.

---

## 🔹 2️⃣ Basic Example

```cpp
static_assert(sizeof(int) == 4, "int must be 4 bytes");
```

If condition fails → compilation error:

```
error: static assertion failed: int must be 4 bytes
```

---

## 🔹 3️⃣ Why Was `static_assert` Introduced?

Before C++11, compile-time checks were ugly:

```cpp
typedef char check[sizeof(int) == 4 ? 1 : -1];
```

Hard to read and poor error messages.

`static_assert` made this clean and readable.

---

## 🔹 4️⃣ Compile-Time vs Runtime Assertion

| Feature                 | assert     | static_assert |
| ----------------------- | ---------- | ------------- |
| Checked at runtime      | ✅          | ❌             |
| Checked at compile time | ❌          | ✅             |
| Can be disabled         | ✅ (NDEBUG) | ❌             |
| Works in templates      | ❌          | ✅             |

Example runtime assert:

```cpp
#include <cassert>
assert(x > 0);
```

Runs during execution.

Static assert:

```cpp
static_assert(sizeof(double) == 8, "Unexpected size");
```

Checked before program runs.

---

## 🔹 5️⃣ Using static_assert with constexpr

```cpp
constexpr int square(int x) {
    return x * x;
}

static_assert(square(4) == 16, "Math error");
```

Compile-time verification.

---

## 🔹 6️⃣ static_assert in Templates (Very Important)

This is where it shines.

Example:

```cpp
#include <type_traits>

template<typename T>
void foo(T value) {
    static_assert(std::is_integral<T>::value,
                  "T must be integral type");
}
```

Now:

```cpp
foo(10);     // OK
foo(3.14);   // Compile-time error
```

This prevents invalid template instantiation.

---

## 🔹 7️⃣ Why It’s Important in Generic Programming

Templates can generate invalid code silently.

`static_assert` ensures constraints.

Example:

```cpp
template<typename T>
class MyContainer {
    static_assert(std::is_copy_constructible<T>::value,
                  "T must be copyable");
};
```

Safer generic code.

---

## 🔹 8️⃣ static_assert Requires Constant Expression

This works:

```cpp
static_assert(5 > 3, "Math broken");
```

This does NOT:

```cpp
int x = 5;
static_assert(x > 3, "Error");   // ❌ x not constant expression
```

---

## 🔹 9️⃣ Placement Rules

`static_assert` can appear:

✔ Global scope
✔ Inside class
✔ Inside function
✔ Inside template

Example inside class:

```cpp
class Test {
    static_assert(sizeof(int) == 4,
                  "Unexpected int size");
};
```

---

## 🔹 🔟 Advanced Example – Alignment Check

```cpp
struct Data {
    int x;
    double y;
};

static_assert(alignof(Data) % alignof(double) == 0,
              "Alignment issue");
```

Used in low-level systems programming.

---

## 🔹 1️⃣1️⃣ Internal Compiler Behavior

When compiler sees:

```cpp
static_assert(condition, "message");
```

It:

* Evaluates condition at compile time
* If false → emits compile error
* Stops compilation

No runtime code generated.

---

## 🔹 1️⃣2️⃣ Common Interview Trap

```cpp
template<typename T>
void func() {
    static_assert(sizeof(T) > 4, "Too small");
}
```

Important:

`static_assert` inside templates only triggers when template is instantiated.

---

## 🔹 1️⃣3️⃣ static_assert vs SFINAE

Before C++20 concepts:

* SFINAE silently removes overload
* static_assert produces clear error

Better error messages → better developer experience.

---

## 🔹 1️⃣4️⃣ Real-World Use Cases

✔ Enforce type constraints
✔ Validate struct sizes
✔ Check architecture assumptions
✔ Embedded systems
✔ Template debugging
✔ Library APIs

---

## 🔹 1️⃣5️⃣ Interview-Level Explanation

If interviewer asks:

> What is static_assert?

Answer:

> static_assert is a compile-time assertion mechanism introduced in C++11. It evaluates a constant expression at compile time and produces a compilation error if the condition is false. It is commonly used in template programming to enforce type constraints and validate compile-time assumptions, improving safety and error diagnostics.

---

## 🔹 1️⃣6️⃣ Key Takeaway

* No runtime cost
* Safer template programming
* Clear compile-time diagnostics
* Essential for modern C++ libraries

---

