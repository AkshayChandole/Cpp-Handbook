
# [Observer Design Pattern](#observer-design-pattern)

## 📌 Intent (Canonical Definition)

> **Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified automatically.**

In simpler words:

> **Observers subscribe to a subject, and the subject notifies them when something changes.**

---

## 🧠 What Problem Does Observer Solve?

Without Observer:

* Objects are **tightly coupled**
* Subject must **know all dependents**
* Adding/removing listeners requires code changes
* Leads to rigid, hard-to-extend systems

Observer:

* Decouples **state change** from **reaction**
* Allows dynamic subscription/unsubscription
* Supports event-driven architectures

---

## 🔷 Core Roles in Observer Pattern

| Role             | Responsibility                         |
| ---------------- | -------------------------------------- |
| Subject          | Maintains state and notifies observers |
| Observer         | Reacts to subject state changes        |
| ConcreteSubject  | Actual implementation holding state    |
| ConcreteObserver | Actual listeners                       |

---

## ❌ Problem Without Observer (Tight Coupling)

```cpp
class OrderService {
public:
    void placeOrder() {
        // business logic
        sendEmail();
        updateDashboard();
        writeAuditLog();
    }
};
```

Problems:

* Every new reaction → modify `OrderService`
* Violates extensibility
* Hard to test

---

## ✅ Observer Pattern Solution

---

# 🧩 Step-by-Step C++ Implementation

---

## 1️⃣ Observer Interface

```cpp
class Observer {
public:
    virtual void update(int newState) = 0;
    virtual ~Observer() = default;
};
```

---

## 2️⃣ Subject Interface

```cpp
#include <vector>
#include <algorithm>

class Subject {
public:
    virtual void attach(Observer* obs) = 0;
    virtual void detach(Observer* obs) = 0;
    virtual void notify() = 0;
    virtual ~Subject() = default;
};
```

---

## 3️⃣ Concrete Subject

```cpp
class Sensor : public Subject {
    int state{};
    std::vector<Observer*> observers;

public:
    void attach(Observer* obs) override {
        observers.push_back(obs);
    }

    void detach(Observer* obs) override {
        observers.erase(
            std::remove(observers.begin(), observers.end(), obs),
            observers.end()
        );
    }

    void setState(int s) {
        state = s;
        notify();
    }

    int getState() const {
        return state;
    }

    void notify() override {
        for (auto* obs : observers) {
            obs->update(state);
        }
    }
};
```

---

## 4️⃣ Concrete Observers

```cpp
#include <iostream>

class Display : public Observer {
public:
    void update(int newState) override {
        std::cout << "Display updated: " << newState << "\n";
    }
};

class Logger : public Observer {
public:
    void update(int newState) override {
        std::cout << "Logging value: " << newState << "\n";
    }
};
```

---

## 5️⃣ Client Code

```cpp
int main() {
    Sensor sensor;

    Display display;
    Logger logger;

    sensor.attach(&display);
    sensor.attach(&logger);

    sensor.setState(42);
}
```

```cpp
// Output:
// Display updated: 42
// Logging value: 42
```

---

## 🔥 Why This Works

✔️ Subject doesn’t know observer details
✔️ Observers can be added/removed at runtime
✔️ No modification to subject when adding new observers
✔️ Clean separation of concerns

---

## 🔷 Push vs Pull Model (VERY IMPORTANT)

### Push Model (above)

```cpp
obs->update(state);
```

* Subject pushes data
* Simple
* Observers tightly coupled to update signature

---

### Pull Model (alternative)

```cpp
class Observer {
public:
    virtual void update() = 0;
};

void notify() {
    for (auto* obs : observers)
        obs->update();
}
```

Observers call:

```cpp
subject.getState();
```

✔️ More flexible
❌ Extra coupling to subject

---

## 🔷 Lifetime Management (CRITICAL in C++)

### ❌ Dangerous

```cpp
std::vector<Observer*> observers;
```

* Dangling pointer risk

---

### ✅ Safer Option: `std::weak_ptr`

```cpp
std::vector<std::weak_ptr<Observer>> observers;
```

Then lock before notifying.

---

## 🔷 Observer with Smart Pointers (Production-Grade)

```cpp
class Subject {
    std::vector<std::weak_ptr<Observer>> observers;

public:
    void notify(int state) {
        for (auto it = observers.begin(); it != observers.end();) {
            if (auto obs = it->lock()) {
                obs->update(state);
                ++it;
            } else {
                it = observers.erase(it);
            }
        }
    }
};
```

---

## 🔷 Observer Using `std::function` (Modern C++)

```cpp
#include <functional>

class Subject {
    std::vector<std::function<void(int)>> observers;

public:
    void subscribe(std::function<void(int)> fn) {
        observers.push_back(fn);
    }

    void setState(int s) {
        for (auto& fn : observers)
            fn(s);
    }
};
```

✔️ Lightweight

✔️ No inheritance

❌ Harder to unsubscribe selectively

---

## 🔷 Common Observer Pitfalls (INTERVIEW FAVORITES)

❌ Forgetting to detach observers

❌ Dangling observer pointers

❌ Circular notifications

❌ Blocking operations inside `update()`

❌ Order-dependent logic

---

## 🔷 When to Use Observer

✔️ Event systems

✔️ UI updates

✔️ Model–View architectures

✔️ Notifications

✔️ Real-time data feeds

---

## 🔷 When NOT to Use Observer

❌ Tight performance constraints

❌ Simple, linear logic

❌ When reactions are fixed and known

---

## 🔥 Interview One-Liner (MEMORIZE)

> “The Observer pattern defines a one-to-many dependency where observers are notified automatically when the subject’s state changes.”

---
