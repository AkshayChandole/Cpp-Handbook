 # [Decorator Pattern](#decorator-pattern)



## 📌 Intent (Canonical Definition)

> **Attach additional responsibilities to an object dynamically.
> Decorators provide a flexible alternative to subclassing for extending functionality.**

In simple words:

> **Add new behavior to an object at runtime without modifying its class.**

---

## 🧠 What Problem Does Decorator Solve?

Without Decorator:

* You create many subclasses to represent combinations of behaviors
* Leads to **class explosion**
* Hard to maintain

Decorator:

* Uses **composition**
* Behaviors can be stacked dynamically
* Avoids rigid inheritance hierarchies

---

## ❌ Problem Without Decorator (Subclass Explosion)

```cpp
class Coffee {
public:
    virtual double cost() const = 0;
};

class MilkCoffee : public Coffee {};
class SugarCoffee : public Coffee {};
class MilkSugarCoffee : public Coffee {};
class MilkSugarVanillaCoffee : public Coffee {};
```

❌ Exponential growth of subclasses

---

## ✅ Decorator Pattern Solution

---

# 🧩 Key Participants

| Role              | Responsibility  |
| ----------------- | --------------- |
| Component         | Base interface  |
| ConcreteComponent | Core object     |
| Decorator         | Wraps component |
| ConcreteDecorator | Adds behavior   |

---

# ☕ C++ Example: Beverage Customization

---

## 1️⃣ Component Interface

```cpp
class Beverage {
public:
    virtual double cost() const = 0;
    virtual std::string description() const = 0;
    virtual ~Beverage() = default;
};
```

---

## 2️⃣ Concrete Component

```cpp
class Espresso : public Beverage {
public:
    double cost() const override {
        return 50.0;
    }

    std::string description() const override {
        return "Espresso";
    }
};
```

---

## 3️⃣ Base Decorator

```cpp
class BeverageDecorator : public Beverage {
protected:
    std::unique_ptr<Beverage> beverage;

public:
    BeverageDecorator(std::unique_ptr<Beverage> b)
        : beverage(std::move(b)) {}
};
```

---

## 4️⃣ Concrete Decorators

```cpp
class Milk : public BeverageDecorator {
public:
    Milk(std::unique_ptr<Beverage> b)
        : BeverageDecorator(std::move(b)) {}

    double cost() const override {
        return beverage->cost() + 10.0;
    }

    std::string description() const override {
        return beverage->description() + ", Milk";
    }
};

class Sugar : public BeverageDecorator {
public:
    Sugar(std::unique_ptr<Beverage> b)
        : BeverageDecorator(std::move(b)) {}

    double cost() const override {
        return beverage->cost() + 5.0;
    }

    std::string description() const override {
        return beverage->description() + ", Sugar";
    }
};
```

---

## 5️⃣ Client Code

```cpp
int main() {
    std::unique_ptr<Beverage> drink =
        std::make_unique<Milk>(
            std::make_unique<Sugar>(
                std::make_unique<Espresso>()
            )
        );

    std::cout << drink->description() << "\n";
    std::cout << drink->cost() << "\n";
}
```

```cpp
// Output:
// Espresso, Sugar, Milk
// 65
```

---

## 🔥 Why This Works

✔️ Behaviors added at runtime

✔️ No modification to existing classes

✔️ Unlimited combinations

✔️ Follows Open/Closed Principle

---

## 🔷 Decorator vs Inheritance (Key Insight)

| Inheritance     | Decorator            |
| --------------- | -------------------- |
| Static          | Dynamic              |
| Compile-time    | Runtime              |
| Class explosion | Flexible composition |

Decorator favors **composition over inheritance**.

---

## 🔷 Order Matters (IMPORTANT)

```cpp
Milk(Sugar(Espresso)) != Sugar(Milk(Espresso))
```

Decorator order can affect:

* Behavior
* Output
* Performance

---

## 🔷 Transparent vs Non-Transparent Decorators

* **Transparent**: Same interface as component (recommended)
* **Non-transparent**: Adds extra methods (less flexible)

---

## 🔷 Decorator Using References (Alternative)

```cpp
class Decorator : public Component {
protected:
    Component& comp;
};
```

⚠️ Lifetime must be managed externally.

---

## 🔷 Real-World Use Cases

✔️ Logging (file + console + remote)

✔️ Stream I/O (`std::istream` decorators)

✔️ GUI components

✔️ Middleware pipelines

✔️ Authorization layers

---

## 🔥 STL Example (INTERVIEW GOLD)

```cpp
std::ifstream file("data.txt");
std::istream& in = file;
```

`istream` decorators:

* buffering
* formatting
* filtering

---

## 🔥 Interview One-Liner (MEMORIZE)

> “The Decorator pattern allows behavior to be added to an object dynamically by wrapping it, providing a flexible alternative to subclassing.”

---
