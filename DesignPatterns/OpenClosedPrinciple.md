
# [Open / Closed Principle (OCP)](#open-closed-principle-ocp)

## 📌 Definition (Canonical)

> **Software entities (classes, modules, functions) should be *open for extension* but *closed for modification*.**

---

## 🧠 What does this REALLY mean?

* **Open for extension**
  → You should be able to **add new behavior**

* **Closed for modification**
  → You should NOT need to **change existing, tested code**

This reduces:

* Regression bugs
* Re-testing cost
* Risk in production systems

---

## 🔥 Why OCP Exists (Big Picture)

Imagine a class that:

* Is already deployed
* Is already tested
* Is used in many places

If you **edit it every time a new feature comes**, you:

* Break existing behavior
* Increase regression risk
* Violate stability

➡️ OCP says: **extend, don’t edit**

---

## ❌ Classic OCP Violation

### BAD DESIGN

```cpp
#include <iostream>

class DiscountCalculator {
public:
    double calculate(double amount, const std::string& type) {
        if (type == "STUDENT")
            return amount * 0.9;
        else if (type == "SENIOR")
            return amount * 0.8;
        else if (type == "FESTIVAL")
            return amount * 0.85;
        else
            return amount;
    }
};
```

---

## ❌ Why This Violates OCP

Every new discount:

* Requires modifying `DiscountCalculator`
* Adds another `if/else`
* Risks breaking existing logic

➡️ **Class is NOT closed for modification**

---

## ✅ OCP-Compliant Design (Using Polymorphism)

### Step 1: Introduce abstraction

```cpp
class DiscountStrategy {
public:
    virtual ~DiscountStrategy() = default;
    virtual double apply(double amount) const = 0;
};
```

---

### Step 2: Implement extensions

```cpp
class StudentDiscount : public DiscountStrategy {
public:
    double apply(double amount) const override {
        return amount * 0.9;
    }
};

class SeniorDiscount : public DiscountStrategy {
public:
    double apply(double amount) const override {
        return amount * 0.8;
    }
};

class FestivalDiscount : public DiscountStrategy {
public:
    double apply(double amount) const override {
        return amount * 0.85;
    }
};
```

---

### Step 3: Use abstraction

```cpp
class DiscountCalculator {
public:
    double calculate(double amount,
                     const DiscountStrategy& strategy) {
        return strategy.apply(amount);
    }
};
```

---

### Usage

```cpp
int main() {
    DiscountCalculator calc;
    StudentDiscount student;

    std::cout << calc.calculate(1000, student);
}
```

```cpp
// Output:
// 900
```

---

## 🔥 Why This Follows OCP

✔️ New discount → **new class only**
✔️ `DiscountCalculator` never changes
✔️ Existing code untouched
✔️ Polymorphism handles behavior

---

## 🔷 OCP is NOT Just Inheritance (INTERVIEW TRAP)

OCP can be achieved using:

* **Inheritance + polymorphism**
* **Composition**
* **Templates**
* **Function objects**
* **Strategy pattern**
* **Dependency Injection**

---

## 🔷 OCP via Composition (Better than inheritance sometimes)

```cpp
class DiscountCalculator {
    std::function<double(double)> discountFn;

public:
    DiscountCalculator(std::function<double(double)> fn)
        : discountFn(fn) {}

    double calculate(double amount) {
        return discountFn(amount);
    }
};
```

Usage:

```cpp
DiscountCalculator calc(
    [](double amt) { return amt * 0.9; }
);
```

✔️ Still OCP
✔️ No inheritance hierarchy

---

## 🔷 OCP Using Templates (Compile-Time Extension)

```cpp
template <typename DiscountPolicy>
class Calculator {
public:
    double calculate(double amount) {
        return DiscountPolicy::apply(amount);
    }
};
```

```cpp
struct StudentPolicy {
    static double apply(double a) {
        return a * 0.9;
    }
};
```

✔️ Zero runtime cost
❌ Less flexible at runtime

---

## 🔥 OCP and Switch/If Statements

### Rule of Thumb:

> **If you frequently add new cases to an `if/switch`, OCP is violated.**

⚠️ Exception:

* Finite, closed set (e.g., enum states)
* Performance-critical code

---

## 🔷 OCP in Real Systems

### Example: Payment System

❌ BAD:

```cpp
if (type == "CARD") ...
if (type == "UPI") ...
if (type == "NETBANKING") ...
```

✅ GOOD:

```cpp
class PaymentMethod {
public:
    virtual void pay() = 0;
};
```


---

## 🔥 Interview One-Liner (MEMORIZE)

> “The Open/Closed Principle states that software entities should be open for extension but closed for modification, typically achieved through abstractions and polymorphism.”

---
