# [rvalue references](#rvalue-references)

## 🔹 1️⃣ Lvalue vs Rvalue (Foundation)

#### 🔹 Lvalue

An object that has:

* A name
* A memory address
* Can appear on left side of assignment

Example:

```cpp
int x = 10;
```

Here:

* `x` → lvalue
* It has memory location

---

#### 🔹 Rvalue

A temporary value:

* No persistent name
* Usually appears on right side
* Short-lived

Example:

```cpp
10
x + 5
std::string("Hello")
```

These are rvalues.

---

## 🔹 2️⃣ Before C++11 Problem

Consider:

```cpp
std::vector<int> createVector() {
    std::vector<int> v = {1,2,3};
    return v;
}
```

Before C++11:

* Returning `v` causes copy
* Copying large objects is expensive

We needed a way to transfer ownership instead of copying.

---

## 🔹 3️⃣ What is an Rvalue Reference?

Syntax:

```cpp
T&&
```

Example:

```cpp
int&& r = 10;
```

Here:

* `10` is rvalue
* `r` binds to temporary

This is an **rvalue reference**.

---

## 🔹 4️⃣ Why Introduced?

To enable:

✔ Move semantics
✔ Efficient transfer of resources
✔ Avoid unnecessary deep copies

---

## 🔹 5️⃣ Basic Example

```cpp
void foo(int& x) {
    std::cout << "Lvalue reference\n";
}

void foo(int&& x) {
    std::cout << "Rvalue reference\n";
}

int main() {
    int a = 10;

    foo(a);    // Lvalue
    foo(20);   // Rvalue
}
```

Output:

```
Lvalue reference
Rvalue reference
```

---

## 🔹 6️⃣ Move Semantics

The real power of rvalue references.

Example custom class:

```cpp
class MyString {
private:
    char* data;

public:
    MyString(const char* str) {
        data = new char[strlen(str) + 1];
        strcpy(data, str);
    }

    // Move constructor
    MyString(MyString&& other) noexcept {
        data = other.data;
        other.data = nullptr;
    }

    ~MyString() {
        delete[] data;
    }
};
```

When returning temporary:

```cpp
MyString s = MyString("Hello");
```

Instead of copying memory:

* Move constructor steals pointer
* Sets source to nullptr
* No deep copy

Huge performance improvement.

---

## 🔹 7️⃣ Important: Named Rvalue Reference Is Lvalue

This is critical.

```cpp
int&& r = 10;
foo(r);  // Calls lvalue overload!
```

Why?

Because `r` has a name → it's lvalue.

To convert back:

```cpp
foo(std::move(r));
```

---

## 🔹 8️⃣ std::move Explained

`std::move` does NOT move anything.

It just casts:

```cpp
static_cast<T&&>(obj)
```

Example:

```cpp
std::string s = "Hello";
std::string t = std::move(s);
```

Now:

* `t` takes ownership
* `s` becomes valid but unspecified

---

## 🔹 9️⃣ Reference Collapsing Rules (Very Important)

Rule:

| Combination | Result |
| ----------- | ------ |
| T& &        | T&     |
| T& &&       | T&     |
| T&& &       | T&     |
| T&& &&      | T&&    |

This enables perfect forwarding.

---

## 🔹 🔟 Perfect Forwarding (Preview)

```cpp
template<typename T>
void wrapper(T&& arg) {
    process(std::forward<T>(arg));
}
```

Here:

* If arg is lvalue → stays lvalue
* If arg is rvalue → stays rvalue

This is forwarding reference.

---

## 🔹 1️⃣1️⃣ Difference Between Rvalue Reference and Forwarding Reference

```cpp
void foo(int&& x);  // pure rvalue reference

template<typename T>
void foo(T&& x);    // forwarding reference
```

Forwarding reference depends on template deduction.

---

## 🔹 1️⃣2️⃣ Performance Impact

Move avoids:

❌ Deep copy
❌ Memory allocation
❌ Expensive resource duplication

Used heavily in:

* std::vector
* std::string
* std::unique_ptr
* std::shared_ptr

---

## 🔹 1️⃣3️⃣ Common Interview Traps

#### Trap 1:

```cpp
int&& r = 10;
int&& r2 = r;   // ❌ error
```

Because `r` is lvalue.

Correct:

```cpp
int&& r2 = std::move(r);
```

---

#### Trap 2:

```cpp
void foo(int&&);
int x = 5;
foo(x);  // ❌
```

Because x is lvalue.

---

## 🔹 1️⃣4️⃣ Internal Mechanism

Move constructor:

1. Transfers resource pointer
2. Nullifies source
3. Avoids allocation
4. Ensures no double delete

---

## 🔹 1️⃣5️⃣ Interview-Ready Explanation

If interviewer asks:

> What is rvalue reference?

Answer:

> An rvalue reference, introduced in C++11 using `T&&`, allows binding to temporary objects (rvalues). It enables move semantics, where resources can be transferred rather than copied, significantly improving performance. Rvalue references distinguish between lvalues and rvalues at overload resolution and are fundamental to implementing move constructors and move assignment operators.

---

## 🔹 1️⃣6️⃣ Key Takeaways

✔ Syntax: `T&&`
✔ Enables move semantics
✔ Used with std::move
✔ Named rvalue reference becomes lvalue
✔ Foundation for perfect forwarding

---

