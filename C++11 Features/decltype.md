# [decltype](#decltype)

## 🔹 1️⃣ What is `decltype`?

`decltype` is a **compile-time type inspection operator**.

> It tells you the exact type of an expression — without evaluating it.

Syntax:

```cpp
decltype(expression)
```

It does NOT execute the expression.
It only deduces its type.

---

## 🔹 2️⃣ Basic Example

```cpp
int x = 10;
decltype(x) y = 20;   // y is int
```

Here:

* `x` is int
* `decltype(x)` is int
* So `y` becomes int

---

## 🔹 3️⃣ Why Was decltype Introduced?

Because templates need to deduce return types based on expressions.

Before C++11, this was impossible cleanly.

Example problem:

```cpp
template<typename T, typename U>
??? add(T a, U b) {
    return a + b;
}
```

What is return type?

C++11 solution:

```cpp
template<typename T, typename U>
auto add(T a, U b) -> decltype(a + b) {
    return a + b;
}
```

---

## 🔹 4️⃣ Important: decltype Does NOT Behave Like auto

This is critical.

`auto` removes top-level const and references.
`decltype` preserves them exactly.

---

### Example

```cpp
int x = 10;
int& ref = x;

auto a = ref;         // int
decltype(ref) b = x;  // int&
```

Key difference:

* `auto` drops reference
* `decltype` keeps reference

---

## 🔹 5️⃣ The Three decltype Rules (Very Important)

This is where interview questions come from.

#### Rule 1:

If expression is a variable name (id-expression):

```cpp
int x = 10;
decltype(x) a;  // int
```

Type is declared type of variable.

---

#### Rule 2:

If expression is a function call:

```cpp
int foo();
decltype(foo()) a;  // int
```

Type is function return type.

---

#### Rule 3 (Critical):

If expression is wrapped in parentheses → reference behavior changes.

Example:

```cpp
int x = 10;

decltype(x) a = x;     // int
decltype((x)) b = x;   // int&
```

Why?

Because:

```cpp
(x)
```

is an lvalue expression.

Rule:

> If expression is an lvalue → decltype gives T&

---

## 🔥 6️⃣ Deep Dive: Lvalue Rule

```cpp
int x = 10;

decltype(x) a;     // int
decltype((x)) b = x;  // int&
```

Explanation:

* `x` → declared type → int
* `(x)` → expression → lvalue → int&

This is one of the most asked interview traps.

---

## 🔹 7️⃣ decltype with const

```cpp
const int x = 10;

decltype(x) a = 20;   // const int
```

Unlike auto:

```cpp
auto b = x;   // int (const removed)
```

`decltype` preserves const.

---

## 🔹 8️⃣ decltype with Expressions

```cpp
int a = 5;
double b = 3.2;

decltype(a + b) c;  // double
```

Because `a + b` results in double.

---

## 🔹 9️⃣ decltype and Templates (Most Important Use Case)

Example:

```cpp
template<typename T>
auto square(T x) -> decltype(x * x) {
    return x * x;
}
```

Without decltype, return type deduction wasn’t possible in C++11.

(C++14 allows auto return deduction.)

---

## 🔹 🔟 decltype and Trailing Return Type

Because function parameters aren't visible before function body:

```cpp
template<typename T, typename U>
auto add(T a, U b) -> decltype(a + b) {
    return a + b;
}
```

Return type comes after parameter list.

---

## 🔹 1️⃣1️⃣ decltype(auto) (C++14 but Important)

Mentioning for completeness:

```cpp
decltype(auto) func();
```

Preserves reference exactly.

---

## 🔹 1️⃣2️⃣ Real-World Example

```cpp
std::vector<int> v;

decltype(v.begin()) it = v.begin();
```

Instead of writing:

```cpp
std::vector<int>::iterator it = v.begin();
```

---

## 🔹 1️⃣3️⃣ auto vs decltype Summary

| Feature                          | auto | decltype |
| -------------------------------- | ---- | -------- |
| Removes top-level const          | ✅    | ❌        |
| Removes references               | ✅    | ❌        |
| Requires initializer             | ✅    | ❌        |
| Can inspect arbitrary expression | ❌    | ✅        |

---

## 🔹 1️⃣4️⃣ Interview Tricky Questions

#### Question:

```cpp
int x = 10;
decltype((x)) a = x;
```

What is type of `a`?

Answer:

```cpp
int&
```

---

#### Question:

```cpp
int foo();
decltype(foo()) a;
```

Type?

Answer:

Return type of foo.

---

## 🔹 1️⃣5️⃣ Internal Compiler View

Compiler performs:

* Expression analysis
* Determines value category (lvalue, rvalue)
* Applies rule:

If expression is lvalue → type becomes T&

---

## 🔹 1️⃣6️⃣ Why decltype Is Powerful

It enables:

✔ Perfect return type deduction
✔ Template metaprogramming
✔ Generic programming
✔ Expression-based type deduction

Modern C++ relies heavily on it.

---

## 🔹 Final Interview-Ready Explanation

> decltype is a compile-time operator introduced in C++11 that yields the exact declared type of an expression without evaluating it. Unlike auto, decltype preserves const and reference qualifiers. If the expression is an lvalue, decltype yields a reference type. It is primarily used in templates and trailing return types to deduce return types based on expressions.

---
