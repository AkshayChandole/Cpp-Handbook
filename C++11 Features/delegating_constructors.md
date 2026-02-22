# [Delegating Constructors](#delegating-constructors)

## 🔹 1️⃣ What Problem Did It Solve?

Before C++11:

You had to duplicate initialization logic.

Example (Pre-C++11):

```cpp id="hmubot"
class Person {
private:
    std::string name;
    int age;

public:
    Person(std::string n, int a)
        : name(n), age(a) {}

    Person(std::string n)
        : name(n), age(0) {}   // duplicated logic

    Person()
        : name("Unknown"), age(0) {}  // duplicated logic again
};
```

Problem:

* Repeated initialization logic
* Hard to maintain
* Bug-prone

---

## 🔹 2️⃣ What Are Delegating Constructors?

C++11 allows one constructor to call another constructor of the same class.

Syntax:

```cpp id="5n2emx"
ClassName(args) : ClassName(other_args) {}
```

---

## 🔹 3️⃣ Example

```cpp id="38ot3t"
#include <iostream>
#include <string>

class Person {
private:
    std::string name;
    int age;

public:
    // Primary constructor
    Person(std::string n, int a)
        : name(n), age(a) {
        std::cout << "Main constructor\n";
    }

    // Delegating constructor
    Person(std::string n)
        : Person(n, 0) {
        std::cout << "Delegating constructor 1\n";
    }

    Person()
        : Person("Unknown", 0) {
        std::cout << "Delegating constructor 2\n";
    }
};
```

---

## 🔹 4️⃣ Execution Order

If you write:

```cpp id="3bm9tv"
Person p;
```

Execution:

1. Calls `Person()`
2. Delegates to `Person("Unknown", 0)`
3. That constructor runs first
4. Then control returns to delegating constructor body

Output:

```id="fxcn4k"
Main constructor
Delegating constructor 2
```

Important rule:

> Target constructor runs first.

---

## 🔹 5️⃣ Why Is This Useful?

✔ Avoid code duplication
✔ Centralize initialization
✔ Maintain invariants
✔ Cleaner design

---

## 🔹 6️⃣ Important Rule

A delegating constructor:

> Must call another constructor in the initializer list.

You cannot mix:

```cpp id="j9sl6h"
Person(std::string n)
    : name(n), Person(n, 0) {}  // ❌ Invalid
```

Only one constructor call allowed in initializer list.

---

## 🔹 7️⃣ Delegation Chain Example

```cpp id="jotcct"
class Test {
public:
    Test() : Test(0) {
        std::cout << "Default\n";
    }

    Test(int x) : Test(x, 0) {
        std::cout << "One param\n";
    }

    Test(int x, int y) {
        std::cout << "Two param\n";
    }
};
```

Calling:

```cpp id="s9a8r6"
Test t;
```

Order:

```id="mgn4wd"
Two param
One param
Default
```

---

## 🔹 8️⃣ Constructor Cannot Delegate to Itself

```cpp id="6zhkuy"
class A {
public:
    A() : A() {}   // ❌ infinite recursion
};
```

Compiler error.

---

## 🔹 9️⃣ Memory Behavior

Delegating constructors do NOT:

* Create multiple objects
* Call constructor twice on same object

It’s still one object being initialized.

Flow:

1. Memory allocated
2. Target constructor initializes object
3. Control returns to delegating constructor body

---

## 🔹 🔟 Interaction with Inheritance

Delegating constructors work only within same class.

They do NOT replace base class constructor calls.

Example:

```cpp id="91gvd5"
class Base {
public:
    Base(int x) {}
};

class Derived : public Base {
public:
    Derived() : Base(10) {}  // must call base explicitly
};
```

Delegation does not affect base class rules.

---

## 🔹 1️⃣1️⃣ Common Interview Trap

Question:

Can delegating constructor call a private constructor?

Answer:

Yes, because it’s inside same class.

---

## 🔹 1️⃣2️⃣ Advanced Design Insight

Best practice:

* Create one “primary” constructor
* All others delegate to it
* Ensures single point of truth

Example pattern:

```cpp id="nbb6m0"
class Config {
public:
    Config(int port, bool debug);

    Config(int port)
        : Config(port, false) {}

    Config()
        : Config(8080, false) {}
};
```

---

## 🔹 1️⃣3️⃣ Why This Improves Safety

Without delegation:

* Members might be initialized differently
* Bugs from inconsistent defaults

With delegation:

* All paths go through one constructor

---

## 🔹 1️⃣4️⃣ Interview-Level Explanation

If interviewer asks:

> What are delegating constructors?

Answer:

> Delegating constructors, introduced in C++11, allow one constructor to call another constructor of the same class using the initializer list. This avoids code duplication and centralizes initialization logic. The delegated-to constructor executes first, and then control returns to the delegating constructor body.

---

## 🔹 1️⃣5️⃣ Key Takeaways

✔ Introduced in C++11
✔ Avoids duplicated initialization logic
✔ Must use initializer list
✔ Target constructor executes first
✔ Improves maintainability

---
