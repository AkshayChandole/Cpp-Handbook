# [constexpr](#constexpr)

`constexpr` means **can be evaluated at compile time**.

It is stronger than `const`.

It guarantees that a value or function **can produce a constant expression** if given constant inputs.

---

## 🔹 1️⃣ constexpr Variable

```cpp
constexpr int x = 10;
```

Rules:

* Must be initialized
* Must be known at compile time

❌ Not allowed:

```cpp
int n = 5;
constexpr int x = n; // error (n not compile-time constant)
```

---

### Compile-Time Usage Example

```cpp
constexpr int size = 5;
int arr[size];  // OK
```

If you use `const` instead:

```cpp
int n = 5;
const int size = n;  // runtime constant
int arr[size];       // ❌ may fail
```

---

## 🔹 2️⃣ constexpr vs const

| const          | constexpr                                             |
| -------------- | ----------------------------------------------------- |
| Read-only      | Compile-time constant                                 |
| May be runtime | Must be compile-time (if used as constant expression) |
| Less strict    | More strict                                           |

Example:

```cpp
const int x = 5;        // maybe runtime
constexpr int y = 5;    // guaranteed compile-time
```

---

## 🔹 3️⃣ constexpr Functions

A function marked `constexpr` can run at compile time **if given constant arguments**.

```cpp
constexpr int square(int x) {
    return x * x;
}
```

Compile-time evaluation:

```cpp
constexpr int val = square(5);
```

Runtime evaluation:

```cpp
int n;
cin >> n;
int val2 = square(n); // runs at runtime
```

👉 Same function, two behaviors.

---

## 🔹 4️⃣ Rules for constexpr Function

C++11 restrictions:

* Single return statement
* No loops
* No local variables

C++14+ relaxed:

* Can use loops
* Can use local variables
* Can have branches

Example (C++14+):

```cpp
constexpr int factorial(int n) {
    int result = 1;
    for (int i = 1; i <= n; ++i)
        result *= i;
    return result;
}
```

---

## 🔹 5️⃣ constexpr with Class

### constexpr Constructor

```cpp
class Point {
public:
    int x, y;

    constexpr Point(int a, int b) : x(a), y(b) {}

    constexpr int sum() const {
        return x + y;
    }
};
```

Usage:

```cpp
constexpr Point p(2, 3);
constexpr int s = p.sum();
```

---

## 🔹 6️⃣ constexpr Objects

```cpp
constexpr int x = 10;
constexpr Point p(1,2);
```

Conditions:

* Class must have constexpr constructor
* All members must be literal types

---

## 🔹 7️⃣ Literal Types Requirement

To be used in constexpr:

* No virtual functions
* Trivial destructor
* All members must be literal types

---

## 🔹 8️⃣ constexpr and if (C++17)

`if constexpr` enables compile-time branching.

```cpp
template<typename T>
void print(T value) {
    if constexpr (std::is_integral<T>::value)
        cout << "Integer\n";
    else
        cout << "Not integer\n";
}
```

👉 Branch removed at compile time.

---

## 🔹 9️⃣ constexpr and std::array

```cpp
constexpr std::array<int, 3> arr = {1,2,3};
```

Compile-time container.

---

## 🔹 🔟 constexpr and Lambda (C++17+)

```cpp
constexpr auto add = [](int a, int b) {
    return a + b;
};

constexpr int result = add(2,3);
```

---

## 🔹 1️⃣1️⃣ constexpr and static members

```cpp
class A {
public:
    static constexpr int x = 10;
};
```

No separate definition needed (C++17 inline variables).

---

## 🔹 1️⃣2️⃣ constexpr vs consteval (C++20)

| constexpr                     | consteval                |
| ----------------------------- | ------------------------ |
| Can run at compile OR runtime | Must run at compile time |

```cpp
consteval int square(int x) {
    return x * x;
}
```

Must be compile-time evaluated.

---

## 🔹 1️⃣3️⃣ constexpr vs constinit (C++20)

`constinit` ensures static initialization at compile time.

```cpp
constinit int x = 10;
```

Prevents dynamic initialization.

---

## 🔹 1️⃣4️⃣ Performance Advantage

Compile-time computation:

* Zero runtime cost
* No stack usage
* No function call overhead

Example:

```cpp
constexpr int val = factorial(5);
```

Compiler replaces with:

```cpp
int val = 120;
```

---

## 🔹 1️⃣5️⃣ Compile-Time Metaprogramming

Before C++11:
Used template metaprogramming.

Now:

```cpp
constexpr int power(int base, int exp) {
    return exp == 0 ? 1 : base * power(base, exp - 1);
}
```

Cleaner than template recursion.

---

## 🔹 1️⃣6️⃣ When constexpr Fails

Not allowed:

* Dynamic memory allocation (before C++20 limited support)
* Virtual functions
* Non-literal types
* Static variables inside function (before C++23)

---

## 🔹 1️⃣7️⃣ Real Interview Trick

```cpp
constexpr int x = 10;
int arr[x]; // OK

const int y = 10;
int arr2[y]; // may work but not guaranteed
```

Because only constexpr guarantees constant expression.

---

## 🔹 1️⃣8️⃣ constexpr + Templates

```cpp
template<int N>
constexpr int cube() {
    return N * N * N;
}

constexpr int val = cube<3>();
```

Compile-time template computation.

---

## 🔹 1️⃣9️⃣ Best Practices

✔ Use `constexpr` for compile-time constants
✔ Use for utility math functions
✔ Use in embedded / performance-critical code
✔ Prefer `constexpr` over `#define`
✔ Use `if constexpr` in templates

---

## 🔥 Ultimate Summary

`constexpr` is about **compile-time computation and stronger guarantees**.

* `const` → read-only
* `constexpr` → compile-time constant
* `consteval` → must compile-time
* `constinit` → compile-time initialization

---
