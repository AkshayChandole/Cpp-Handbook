
# [Liskov Substitution Principle (LSP)](#liskov-substitution-principle-lsp)

## 📌 Formal Definition

> **Objects of a derived class must be substitutable for objects of the base class without altering the correctness of the program.**

In simple words:

> If `B` is a subtype of `A`, then **any code that works with `A` must also work correctly with `B`**.

---

## 🧠 What does “substitutable” REALLY mean?

It does **not** mean:

* Same method names ❌
* Same function signatures ❌

It means:

> **Same expected behavior, guarantees, and constraints**

---

## 🔥 Core Idea of LSP

When a function accepts a **base class**, it relies on:

* Certain **behavioral guarantees**
* Certain **contracts**

A derived class **must not break those expectations**.

---

## 🔷 Think in Terms of CONTRACTS (Very Important)

Every base class defines a **contract**:

### Contract consists of:

1. **Preconditions** – what must be true before a function is called
2. **Postconditions** – what must be true after it returns
3. **Invariants** – what always remains true

### LSP Rules:

* Derived class **must not strengthen preconditions**
* Derived class **must not weaken postconditions**
* Derived class **must preserve invariants**

---

## ❌ Classic LSP Violation Example

### Problem Setup

```cpp
class Rectangle {
protected:
    int width, height;

public:
    virtual void setWidth(int w) { width = w; }
    virtual void setHeight(int h) { height = h; }

    int area() const {
        return width * height;
    }
};
```

This base class **implicitly promises**:

* `setWidth(w)` sets width to `w`
* `setHeight(h)` sets height to `h`
* Width and height are independent

---

### Derived Class That Breaks the Contract

```cpp
class Square : public Rectangle {
public:
    void setWidth(int w) override {
        width = height = w;
    }

    void setHeight(int h) override {
        width = height = h;
    }
};
```

---

### Client Code

```cpp
void resize(Rectangle& r) {
    r.setWidth(5);
    r.setHeight(10);

    if (r.area() != 50)
        std::cout << "LSP violated\n";
}

int main() {
    Square s;
    resize(s);
}
```

```cpp
// Output:
// LSP violated
```

---

## 🔥 Why This Violates LSP

* Code expects **independent width & height**
* `Square` silently changes behavior
* Program correctness breaks

➡️ **Square is NOT a valid substitute for Rectangle**

---

## 🔷 Key Insight (CRITICAL)

> **Just because something is “is-a” in real life does NOT mean it is “is-a” in code.**

Inheritance is about **behavior**, not vocabulary.

---

## 🔷 Another LSP Violation: Throwing Unexpected Exceptions

### Base class promise

```cpp
class FileReader {
public:
    virtual std::string read() {
        return "data";
    }
};
```

---

### Derived class breaks expectations

```cpp
class NetworkReader : public FileReader {
public:
    std::string read() override {
        throw std::runtime_error("Network down");  // ❌
    }
};
```

Client code expects:

* `read()` always returns data

Derived class:

* Strengthens failure conditions

➡️ **LSP violation**

---

## 🔷 Another LSP Violation: Narrowing Valid Input

### Base class

```cpp
class Printer {
public:
    virtual void print(int copies) {
        // accepts any positive number
    }
};
```

---

### Derived class

```cpp
class LimitedPrinter : public Printer {
public:
    void print(int copies) override {
        if (copies > 10)
            throw std::invalid_argument("Too many copies");
    }
};
```

Base contract:

* Any positive number is valid

Derived:

* Restricts valid input

➡️ **Strengthened precondition → LSP violation**

---

## 🔷 Correct LSP-Compliant Design (Fixing the Rectangle Problem)

### Option 1: Remove inheritance

```cpp
class Rectangle {
public:
    int width, height;
};

class Square {
public:
    int side;
};
```

No inheritance → no violation.

---

### Option 2: Make base class contract explicit

```cpp
class Shape {
public:
    virtual int area() const = 0;
    virtual ~Shape() = default;
};

class Rectangle : public Shape {
    int w, h;
public:
    Rectangle(int w, int h) : w(w), h(h) {}
    int area() const override { return w * h; }
};

class Square : public Shape {
    int side;
public:
    Square(int s) : side(s) {}
    int area() const override { return side * side; }
};
```

Now:

* Both obey `Shape` contract
* Both substitutable

---

## 🔥 Interview One-Liner (MEMORIZE)

> “The Liskov Substitution Principle states that objects of a derived class must be substitutable for base class objects without breaking the correctness or expectations of the program.”

---
