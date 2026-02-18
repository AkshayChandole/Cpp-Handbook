# [Const](#const)

## 🔹 1️⃣ What is `const`?

`const` makes something **read-only after initialization**.

It can be applied to:

* Variables
* Pointers
* References
* Function parameters
* Return types
* Member functions
* Objects
* Static members
* Templates
* Iterators
* Rvalue references
* Casts
* Namespaces
* Global variables
* With `auto`
* With `volatile`
* With `constexpr`
* With `typedef` / `using`

---

## 🔹 2️⃣ Const Variables

```cpp
const int x = 10;
// x = 20; ❌ error
```

Must be initialized at declaration:

```cpp
const int y; // ❌ error
```

---

### Global const

```cpp
const int MAX = 100;
```

By default:

* Global `const` has **internal linkage**

If you want external linkage:

```cpp
extern const int MAX;
```

---

## 🔹 3️⃣ Const and Pointers (Very Important)

### Case 1: Pointer to const

```cpp
int x = 10;
const int* ptr = &x;
// or int const* ptr = &x;

*ptr = 20;   // ❌
ptr = nullptr; // ✅
```

👉 Cannot modify value
👉 Can change pointer

---

### Case 2: Const pointer

```cpp
int x = 10;
int* const ptr = &x;

*ptr = 20;   // ✅
ptr = nullptr; // ❌
```

👉 Cannot change pointer
👉 Can modify value

---

### Case 3: Const pointer to const

```cpp
const int* const ptr = &x;
```

👉 Neither pointer nor value can change

---

#### 🔥 Right-to-left rule

Read declaration from right to left:

```cpp
int const * const ptr;
```

* `ptr` is const pointer
* to const int

---

## 🔹 4️⃣ Const References

```cpp
void print(const int& x) {
    // x = 5; ❌
}
```

Why?

* Avoid copying
* Prevent modification
* Can bind to temporary objects

```cpp
print(5); // works because const ref binds to rvalue
```

---

## 🔹 5️⃣ Const with Member Functions

```cpp
class Person {
    int age;
public:
    int getAge() const {
        return age;
    }
};
```

Rules inside const function:

* Cannot modify member variables
* Cannot call non-const member functions

---

### Const Object Behavior

```cpp
const Person p;
p.getAge(); // ✅
```

Const object can only call const member functions.

---

## 🔹 6️⃣ Const Correctness

Good API design ensures:

* Read-only functions marked const
* Parameters passed as const reference

Bad:

```cpp
void process(string s);
```

Good:

```cpp
void process(const string& s);
```

---

## 🔹 7️⃣ Const Return Types

Primitive:

```cpp
const int get(); // not very useful
```

Object reference:

```cpp
const string& getName() const;
```

Prevents modification:

```cpp
obj.getName() = "abc"; // ❌
```

---

## 🔹 8️⃣ Mutable Keyword

Allows modification inside const function.

```cpp
class Test {
    mutable int counter;
public:
    void update() const {
        counter++;
    }
};
```

Used for:

* Caching
* Logging
* Lazy evaluation

---

## 🔹 9️⃣ Const vs constexpr

| const            | constexpr                 |
| ---------------- | ------------------------- |
| Runtime constant | Compile-time constant     |
| Value fixed      | Evaluated at compile time |

```cpp
const int x = 5;
constexpr int y = 5;
```

`constexpr` guarantees compile-time evaluation.

---

## 🔹 1️⃣0️⃣ Const vs #define

```cpp
#define PI 3.14
const double PI = 3.14;
```

Why const is better?

* Type safe
* Scoped
* Debuggable

---

## 🔹 1️⃣1️⃣ Const with auto

```cpp
const int x = 10;
auto y = x;      // y is int (const removed)
const auto z = x; // z is const int
```

---

## 🔹 1️⃣2️⃣ Top-level vs Low-level const

```cpp
int* const ptr;     // top-level const
const int* ptr;     // low-level const
```

Top-level:

* Applies to object itself

Low-level:

* Applies to pointed data

Important in template deduction.

---

## 🔹 1️⃣3️⃣ Const in STL

```cpp
const vector<int> v = {1,2,3};
v[0] = 10; // ❌
```

Const iterators:

```cpp
vector<int>::const_iterator it;
```

C++11:

```cpp
for (const auto& x : v)
```

---

## 🔹 1️⃣4️⃣ Const and Rvalue References

```cpp
void func(const string&& s); // rare usage
```

Usually use:

```cpp
void func(const string&);
```

Because const rvalue ref prevents moving.

---

## 🔹 1️⃣5️⃣ Const and Move Semantics

You cannot move from const object.

```cpp
const string s = "hello";
string s2 = std::move(s); // copy, not move
```

Move constructor requires non-const rvalue.

---

## 🔹 1️⃣6️⃣ Const_cast

Used to remove constness.

```cpp
const int x = 10;
int* p = const_cast<int*>(&x);
```

⚠ Dangerous:
Modifying originally const object → undefined behavior.

Valid only when original object wasn’t const.

---

## 🔹 1️⃣7️⃣ Const and Function Overloading

```cpp
class Test {
public:
    void print() {}
    void print() const {}
};
```

Called based on object constness.

---

## 🔹 1️⃣8️⃣ Const in Templates

```cpp
template<typename T>
void func(const T& value);
```

Template deduction handles const carefully.

---

## 🔹 1️⃣9️⃣ Const Static Members

```cpp
class A {
public:
    static const int x = 10;
};
```

If non-integral:

```cpp
static const string name;
```

Needs definition outside class.

---

## 🔹 2️⃣0️⃣ Const in Function Pointers

```cpp
void func(int);
void (* const ptr)(int) = func;
```

Pointer is const.

---

## 🔹 2️⃣1️⃣ Const Volatile

```cpp
const volatile int x;
```

Used in embedded systems:

* const → read-only
* volatile → may change externally

---

## 🔹 2️⃣2️⃣ Const in Namespaces

```cpp
namespace config {
    const int value = 10;
}
```

Scoped constant.

---

## 🔹 2️⃣3️⃣ Const with Arrays

```cpp
const int arr[3] = {1,2,3};
```

Cannot modify elements.

---

## 🔹 2️⃣4️⃣ Const in Class Constructors

```cpp
class A {
    const int x;
public:
    A(int val) : x(val) {}
};
```

Const members must be initialized using initializer list.

---

## 🔹 2️⃣5️⃣ Why Const Matters (Senior Level)

Improves:

* Code safety
* Compiler optimization
* Thread-safety reasoning
* API clarity
* Intent expression
* Maintainability

---

## 🔹 2️⃣6️⃣ Interview Tricky Question

```cpp
int x = 10;
const int* p = &x;
x = 20; // valid?
```

✅ Yes.

Because only pointer is restricted, not original variable.

---

## 🔹 2️⃣7️⃣ Best Practices

✔ Use const wherever possible
✔ Make getters const
✔ Pass large objects as const reference
✔ Prefer constexpr for compile-time constants
✔ Avoid const_cast unless absolutely necessary

---

