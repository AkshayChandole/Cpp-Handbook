
# [Interface Segregation Principle (ISP)](#interface-segregation-principle)

## 📌 Formal Definition

> **Clients should not be forced to depend on interfaces they do not use.**

In simpler words:

> **Many small, specific interfaces are better than one large, general-purpose interface.**

---

## 🧠 What is an “Interface” in C++?

In C++, an interface is typically:

* An **abstract base class**
* Contains **only pure virtual functions**
* No data members (or minimal)

```cpp
class Device {
public:
    virtual void start() = 0;
    virtual void stop() = 0;
    virtual ~Device() = default;
};
```

---

## 🔥 Core Idea of ISP

ISP is about **dependency hygiene**.

A class should:

* Depend **only on methods it actually uses**
* Not be affected when unrelated methods change

---

## ❌ Classic ISP Violation (Fat Interface)

### BAD DESIGN

```cpp
class Machine {
public:
    virtual void print() = 0;
    virtual void scan() = 0;
    virtual void fax() = 0;
    virtual ~Machine() = default;
};
```

---

### Problematic Implementation

```cpp
class Printer : public Machine {
public:
    void print() override {
        std::cout << "Printing\n";
    }

    void scan() override {
        // ❌ Not supported
        throw std::runtime_error("Scan not supported");
    }

    void fax() override {
        // ❌ Not supported
        throw std::runtime_error("Fax not supported");
    }
};
```

---

## 🔥 Why This Violates ISP

* `Printer` is **forced to implement methods it does not support**
* Dummy implementations or exceptions appear
* Changes to `scan()` or `fax()` force recompilation of `Printer`
* Client code cannot trust interface behavior

➡️ **Client is forced to depend on unused functionality**

---

## 🔷 Correct ISP-Compliant Design

### Step 1: Split interfaces by responsibility

```cpp
class Printable {
public:
    virtual void print() = 0;
    virtual ~Printable() = default;
};

class Scannable {
public:
    virtual void scan() = 0;
    virtual ~Scannable() = default;
};

class Faxable {
public:
    virtual void fax() = 0;
    virtual ~Faxable() = default;
};
```

---

### Step 2: Implement only what is supported

```cpp
class SimplePrinter : public Printable {
public:
    void print() override {
        std::cout << "Printing\n";
    }
};

class AllInOneMachine : public Printable,
                        public Scannable,
                        public Faxable {
public:
    void print() override { std::cout << "Printing\n"; }
    void scan() override { std::cout << "Scanning\n"; }
    void fax() override  { std::cout << "Faxing\n"; }
};
```

---

## 🔥 Why This Follows ISP

✔️ No forced implementation
✔️ No dummy methods
✔️ Clients depend only on needed behavior
✔️ Interfaces are stable and focused

---

## 🔷 ISP from the CLIENT Perspective (CRITICAL)

### Bad client

```cpp
void process(Machine& m) {
    m.print();
}
```

Even though it only prints, it depends on:

* scan()
* fax()

➡️ Tight coupling

---

### Good client

```cpp
void process(Printable& p) {
    p.print();
}
```

✔️ Minimal dependency
✔️ Safe and expressive

---

## 🔷 ISP and Build-Time Dependency (C++ Specific)

Fat interfaces cause:

* Larger headers
* More recompilation
* Slower builds

ISP:

* Reduces header coupling
* Improves incremental builds

🔥 Very important in large C++ codebases (Autodesk-scale)

---

## 🔷 Another ISP Violation Example (Realistic)

```cpp
class UserService {
public:
    virtual void login() = 0;
    virtual void logout() = 0;
    virtual void deleteUser() = 0;
};
```

A read-only client is forced to depend on destructive methods.

---

### ISP-Compliant Fix

```cpp
class AuthService {
public:
    virtual void login() = 0;
    virtual void logout() = 0;
};

class UserAdminService {
public:
    virtual void deleteUser() = 0;
};
```

---

## 🔷 ISP and Exceptions (Important Detail)

If a class:

* Implements a method
* Throws `NotSupportedException`

➡️ That is a **red flag** for ISP violation.

---

## 🔥 How to Identify ISP Violations (INTERVIEW TOOL)

Ask:

1. Does this class implement methods it doesn’t need?
2. Are there “not supported” methods?
3. Do clients depend on unused functions?
4. Does changing one method break many clients?
5. Is this interface growing over time?

If YES → ISP violation.

---

## 🔥 ISP and Multiple Inheritance (C++ Specific)

ISP often leads to:

```cpp
class A : public Interface1, public Interface2 {};
```

This is:
✔️ Safe
✔️ Recommended
✔️ Controlled MI (interfaces only)

---


## 🔥 Interview One-Liner (MEMORIZE)

> “The Interface Segregation Principle states that clients should not be forced to depend on interfaces they do not use, encouraging small, focused interfaces.”

---

Say **go ahead** when ready 👍
