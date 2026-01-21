
# [Strategy Design Pattern](#strategy-design-pattern)

## 📌 Intent (Canonical Definition)

> **Define a family of algorithms, encapsulate each one, and make them interchangeable.
> Strategy lets the algorithm vary independently from the clients that use it.**

In simple words:

> **Select an algorithm at runtime without changing the client code.**

---

## 🧠 What Problem Does Strategy Solve?

Without Strategy:

* Large `if/else` or `switch` blocks
* Hard-coded algorithms
* Adding new behavior requires modifying existing code
* Tight coupling between logic and algorithm

Strategy:

* Encapsulates algorithms
* Eliminates conditionals
* Makes behavior pluggable and extensible

---

## ❌ Problem Without Strategy

```cpp
class PaymentService {
public:
    void pay(double amount, const std::string& type) {
        if (type == "CARD") {
            std::cout << "Paid by card\n";
        } else if (type == "UPI") {
            std::cout << "Paid by UPI\n";
        } else if (type == "NETBANKING") {
            std::cout << "Paid by net banking\n";
        }
    }
};
```

Problems:

* Every new payment method → modify class
* Code grows and becomes fragile
* Violates extensibility

---

## ✅ Strategy Pattern Solution

---

# 🧩 Step-by-Step C++ Implementation

---

## 1️⃣ Strategy Interface

```cpp
class PaymentStrategy {
public:
    virtual void pay(double amount) = 0;
    virtual ~PaymentStrategy() = default;
};
```

---

## 2️⃣ Concrete Strategies

```cpp
class CardPayment : public PaymentStrategy {
public:
    void pay(double amount) override {
        std::cout << "Paid " << amount << " using Card\n";
    }
};

class UpiPayment : public PaymentStrategy {
public:
    void pay(double amount) override {
        std::cout << "Paid " << amount << " using UPI\n";
    }
};

class NetBankingPayment : public PaymentStrategy {
public:
    void pay(double amount) override {
        std::cout << "Paid " << amount << " using Net Banking\n";
    }
};
```

---

## 3️⃣ Context Class

```cpp
class PaymentContext {
    PaymentStrategy* strategy;

public:
    PaymentContext(PaymentStrategy* s) : strategy(s) {}

    void setStrategy(PaymentStrategy* s) {
        strategy = s;
    }

    void execute(double amount) {
        strategy->pay(amount);
    }
};
```

---

## 4️⃣ Client Code

```cpp
int main() {
    CardPayment card;
    UpiPayment upi;

    PaymentContext context(&card);
    context.execute(1000);

    context.setStrategy(&upi);
    context.execute(500);
}
```

```cpp
// Output:
// Paid 1000 using Card
// Paid 500 using UPI
```

---

## 🔥 Why This Works

✔️ Algorithms are interchangeable

✔️ No `if/else` logic

✔️ Easy to add new strategies

✔️ Context code remains unchanged

---

## 🔷 Ownership & Lifetime (C++ Specific)

### ❌ Raw pointer risks

```cpp
PaymentStrategy* strategy;
```

Risk:

* Dangling pointer

---

### ✅ Safer using `unique_ptr`

```cpp
class PaymentContext {
    std::unique_ptr<PaymentStrategy> strategy;

public:
    void setStrategy(std::unique_ptr<PaymentStrategy> s) {
        strategy = std::move(s);
    }
};
```

---

## 🔷 Strategy Using `std::function` (Modern C++)

```cpp
class PaymentContext {
    std::function<void(double)> strategy;

public:
    void setStrategy(std::function<void(double)> s) {
        strategy = s;
    }

    void execute(double amount) {
        strategy(amount);
    }
};
```

Usage:

```cpp
context.setStrategy(
    [](double amt) { std::cout << "Paid " << amt << " using Wallet\n"; }
);
```

✔️ Lightweight

✔️ No inheritance

❌ Harder to share state

---

## 🔷 Strategy vs Inheritance (Key Insight)

Strategy:

* Uses **composition**
* Behavior chosen at runtime

Inheritance:

* Behavior fixed at compile time

Strategy avoids:

* Class explosion
* Rigid hierarchies

---

## 🔷 When to Use Strategy

✔️ Many interchangeable algorithms

✔️ Runtime selection needed

✔️ Avoiding conditional logic

✔️ Want open-ended extensibility

---

## 🔷 When NOT to Use Strategy

❌ Only one algorithm

❌ Behavior never changes

❌ Overhead unacceptable

---


## 🔥 Interview One-Liner (MEMORIZE)

> “The Strategy pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable at runtime.”

---
