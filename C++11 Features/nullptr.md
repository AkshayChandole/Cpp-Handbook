# [nullptr](#nullptr)

## 🔹 1️⃣ What is `nullptr`?

`nullptr` is a keyword introduced in C++11 representing a **null pointer literal**.

It replaces:

```cpp
NULL
0
```

It has its own type:

```cpp
std::nullptr_t
```

---

## 🔹 2️⃣ Why Was `nullptr` Needed?

Before C++11, we used:

```cpp
int* p = NULL;  // or 0
```

But:

```cpp
NULL
```

is typically defined as:

```cpp
#define NULL 0
```

Which is just an integer.

This caused ambiguity.

---

## 🔥 3️⃣ Classic Problem Without `nullptr`

```cpp
void foo(int);
void foo(char*);

foo(NULL);
```

What happens?

Since `NULL` is `0`, it matches:

```cpp
foo(int);
```

NOT:

```cpp
foo(char*);
```

This is dangerous.

---

## 🔹 4️⃣ How `nullptr` Fixes This

```cpp
void foo(int);
void foo(char*);

foo(nullptr);  // Calls foo(char*)
```

Because:

* `nullptr` is not an integer
* It converts only to pointer types

---

## 🔹 5️⃣ Type of nullptr

```cpp
decltype(nullptr)
```

is:

```cpp
std::nullptr_t
```

You can even declare:

```cpp
std::nullptr_t np = nullptr;
```

---

## 🔹 6️⃣ nullptr Conversion Rules

`nullptr` can convert to:

✔ Any raw pointer type
✔ Any pointer-to-member type

But NOT to:

❌ integer types

Example:

```cpp
int* p = nullptr;  // OK
int x = nullptr;   // ❌ Compile error
```

---

## 🔹 7️⃣ Comparison with NULL and 0

| Feature       | 0 | NULL | nullptr |
| ------------- | - | ---- | ------- |
| Type-safe     | ❌ | ❌    | ✅       |
| Overload-safe | ❌ | ❌    | ✅       |
| Separate type | ❌ | ❌    | ✅       |

---

## 🔹 8️⃣ Overload Resolution Example (Very Important)

```cpp
void foo(int);
void foo(int*);

foo(0);         // calls foo(int)
foo(NULL);      // calls foo(int)
foo(nullptr);   // calls foo(int*)
```

Interviewers love this question.

---

## 🔹 9️⃣ nullptr in Templates

```cpp
template<typename T>
void func(T);

func(nullptr);
```

`T` becomes:

```cpp
std::nullptr_t
```

---

## 🔹 🔟 nullptr in Boolean Context

```cpp
int* p = nullptr;

if (p) {
    // not executed
}
```

It behaves like null pointer.

---

## 🔹 1️⃣1️⃣ Internal Representation

Internally:

* Compiler treats `nullptr` as special literal
* Type = `std::nullptr_t`
* Zero-sized type
* Implicitly convertible to any pointer

But NOT stored as integer 0 in type system.

---

## 🔹 1️⃣2️⃣ nullptr and Smart Pointers

```cpp
std::shared_ptr<int> sp = nullptr;
```

Works because smart pointer has constructor accepting `std::nullptr_t`.

---

## 🔹 1️⃣3️⃣ Why This Improves Safety

Without `nullptr`, this bug could happen:

```cpp
void log(int level);
void log(const char* message);

log(NULL);  // accidentally calls log(int)
```

With nullptr:

```cpp
log(nullptr);  // clearly pointer
```

No ambiguity.

---

## 🔹 1️⃣4️⃣ Advanced: sizeof(nullptr)

```cpp
sizeof(nullptr)
```

Size equals size of `std::nullptr_t` (implementation-defined, typically same as pointer size).

---

## 🔹 1️⃣5️⃣ Interview Trick Question

Q:

```cpp
int* p = nullptr;
if (p == 0) {
}
```

Is this valid?

Answer: Yes.
`nullptr` compares equal to 0.

---

## 🔹 1️⃣6️⃣ Best Practice

Always use:

```cpp
nullptr
```

Never use:

```cpp
NULL
0
```

In modern C++.

---

## 🔹 Final Interview-Ready Explanation

> nullptr is a keyword introduced in C++11 representing a null pointer literal. Unlike NULL or 0, it has its own type, std::nullptr_t, which ensures type-safe pointer usage and correct overload resolution. It can implicitly convert to any pointer type but not to integers, eliminating ambiguity and improving code safety.

---

