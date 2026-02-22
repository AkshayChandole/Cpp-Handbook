# [constexpr](#constexpr)

## 🔹 1️⃣ What is `constexpr`?

`constexpr` means:

> The value (or function) can be evaluated at compile time.

It guarantees that an expression **can** be computed at compile time (if given constant inputs).

---

## 🔹 2️⃣ constexpr Variable

```cpp
constexpr int x = 10;
```

Rules:

* Must be initialized immediately
* Must be initialized with a compile-time constant
* Becomes implicitly `const`

Example:

```cpp
constexpr int a = 5;
int arr[a];   // Valid (size known at compile time)
```

---

## 🔴 Invalid Example

```cpp
int x = 10;
constexpr int y = x;  // ❌ x not compile-time constant
```

---

## 🔹 3️⃣ Why constexpr Was Needed?

Before C++11:

```cpp
#define SIZE 10
const int size = 10;
```

Problems:

* Macros are unsafe
* const wasn’t always guaranteed compile-time
* No function compile-time evaluation

`constexpr` solved this.

---

## 🔹 4️⃣ constexpr Function (C++11 Rules)

In C++11, a constexpr function must:

* Have exactly one return statement
* Contain no loops (in strict C++11)
* Use only literal types
* Be able to evaluate at compile time

Example:

```cpp
constexpr int square(int x) {
    return x * x;
}
```

Usage:

```cpp
constexpr int val = square(5);  // evaluated at compile time
```

---

## 🔹 5️⃣ Runtime vs Compile-Time Behavior

Important concept:

```cpp
int x = 5;
int y = square(x);   // runtime evaluation
```

Even though `square` is constexpr,
if input is not compile-time constant → runtime execution.

So `constexpr` means:

> Can run at compile time, not must.

---

## 🔹 6️⃣ constexpr and Array Sizes

```cpp
constexpr int size() {
    return 10;
}

int arr[size()];   // Valid
```

Without constexpr → invalid.

---

## 🔹 7️⃣ constexpr vs const

| Feature                | const | constexpr |
| ---------------------- | ----- | --------- |
| Compile-time guarantee | ❌     | ✅         |
| Functions allowed      | ❌     | ✅         |
| Stronger guarantee     | ❌     | ✅         |

Example:

```cpp
const int x = rand();   // runtime value allowed
constexpr int y = rand(); // ❌ error
```

---

## 🔹 8️⃣ constexpr Constructors

C++11 allows constexpr constructors.

Example:

```cpp
class Point {
public:
    int x, y;

    constexpr Point(int a, int b)
        : x(a), y(b) {}
};

constexpr Point p(3, 4);
```

Now object `p` is created at compile time.

---

## 🔹 9️⃣ Literal Type Requirement

For constexpr objects:

Type must be:

* Trivial destructor
* All members literal types
* No virtual functions

---

## 🔹 🔟 Compile-Time Evaluation Example

```cpp
constexpr int factorial(int n) {
    return n <= 1 ? 1 : n * factorial(n - 1);
}

constexpr int val = factorial(5);  // 120 at compile time
```

---

## 🔹 1️⃣1️⃣ constexpr and Optimization

Compile-time evaluation:

* Eliminates runtime cost
* Improves performance
* Enables static assertions

---

## 🔹 1️⃣2️⃣ static_assert + constexpr

```cpp
constexpr int square(int x) {
    return x * x;
}

static_assert(square(4) == 16, "Error");
```

Compile-time validation.

---

## 🔹 1️⃣3️⃣ Internal Compiler Behavior

When compiler sees:

```cpp
constexpr int val = square(5);
```

It:

* Evaluates square(5)
* Replaces with literal 25
* No runtime code generated

---

## 🔹 1️⃣4️⃣ Important Limitation in C++11

C++11 constexpr is very restrictive:

* Only one return statement
* No loops
* No local variables

C++14 removed many restrictions.

---

## 🔹 1️⃣5️⃣ Interview Trap

```cpp
constexpr int foo(int x) {
    return x * x;
}

int a = 5;
constexpr int b = foo(a);  // ❌
```

Why?

Because `a` is not compile-time constant.

---

## 🔹 1️⃣6️⃣ Difference Between Compile-Time Constant and Runtime Constant

```cpp
const int x = 10;        // maybe compile-time
constexpr int y = 10;    // guaranteed compile-time
```

If const is initialized with literal, it behaves similar.

But constexpr enforces it.

---

## 🔹 1️⃣7️⃣ Real-World Uses

✔ Template metaprogramming
✔ Compile-time math
✔ Embedded systems
✔ Array sizes
✔ Static configuration
✔ Performance-critical code

---

## 🔹 Final Interview-Ready Explanation

> constexpr in C++11 indicates that a variable or function can be evaluated at compile time. constexpr variables must be initialized with compile-time constants and are implicitly const. constexpr functions can execute at compile time if given constant expressions. It provides stronger guarantees than const and enables compile-time computation, optimization, and static validation.

---

