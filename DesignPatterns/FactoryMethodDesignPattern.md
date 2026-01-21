
# [Factory Method Design Pattern](#factory-method-design-pattern)

## 📌 Intent (Canonical Definition)

> **Define an interface for creating an object, but let subclasses decide which class to instantiate.**

In simple words:

> **Creation logic is separated from usage logic**, and object creation is delegated to a factory method.

---

## 🧠 What Problem Does Factory Method Solve?

Without Factory Method:

* Client code depends on **concrete classes**
* Adding new types requires **changing existing code**
* Violates extensibility and maintainability

Factory Method:

* Encapsulates object creation
* Decouples **what is created** from **how it is used**

---

## ❌ Problem Without Factory Method

### BAD DESIGN

```cpp
class Car {
public:
    virtual void drive() = 0;
    virtual ~Car() = default;
};

class Sedan : public Car {
public:
    void drive() override {
        std::cout << "Driving Sedan\n";
    }
};

class SUV : public Car {
public:
    void drive() override {
        std::cout << "Driving SUV\n";
    }
};

void createAndDrive(const std::string& type) {
    Car* car;

    if (type == "sedan")
        car = new Sedan();
    else if (type == "suv")
        car = new SUV();
    else
        throw std::invalid_argument("Unknown car type");

    car->drive();
    delete car;
}
```

### ❌ Problems

* `if/else` grows with new types
* Client code tightly coupled to concrete classes
* Risk of memory leaks
* Hard to extend

---

## ✅ Factory Method Solution

### Step 1: Define Product Interface

```cpp
class Car {
public:
    virtual void drive() = 0;
    virtual ~Car() = default;
};
```

---

### Step 2: Concrete Products

```cpp
class Sedan : public Car {
public:
    void drive() override {
        std::cout << "Driving Sedan\n";
    }
};

class SUV : public Car {
public:
    void drive() override {
        std::cout << "Driving SUV\n";
    }
};
```

---

### Step 3: Creator (Factory Method)

```cpp
class CarFactory {
public:
    virtual std::unique_ptr<Car> createCar() = 0;
    virtual ~CarFactory() = default;
};
```

---

### Step 4: Concrete Factories

```cpp
class SedanFactory : public CarFactory {
public:
    std::unique_ptr<Car> createCar() override {
        return std::make_unique<Sedan>();
    }
};

class SUVFactory : public CarFactory {
public:
    std::unique_ptr<Car> createCar() override {
        return std::make_unique<SUV>();
    }
};
```

---

### Step 5: Client Code

```cpp
void driveCar(CarFactory& factory) {
    auto car = factory.createCar();
    car->drive();
}
```

```cpp
int main() {
    SedanFactory sedanFactory;
    SUVFactory suvFactory;

    driveCar(sedanFactory);
    driveCar(suvFactory);
}
```

```cpp
// Output:
// Driving Sedan
// Driving SUV
```

---

## 🔥 Why This Works

✔️ Client depends only on **abstraction**
✔️ Adding new car → add new factory, no code change
✔️ Creation logic isolated
✔️ Memory safety via `unique_ptr`

---

## 🔷 Key Components of Factory Method

| Role            | Description                  |
| --------------- | ---------------------------- |
| Product         | Interface of objects created |
| ConcreteProduct | Actual implementations       |
| Creator         | Declares factory method      |
| ConcreteCreator | Implements factory method    |

---

## 🔷 Factory Method vs Simple Factory (INTERVIEW TRAP)

### Simple Factory (NOT a GoF pattern)

```cpp
class CarFactory {
public:
    static std::unique_ptr<Car> create(const std::string& type);
};
```

❌ Centralized `if/else`
❌ Violates extensibility

Factory Method:
✔️ Uses inheritance
✔️ Uses polymorphism
✔️ Truly extensible

---

## 🔷 Factory Method with Parameters

```cpp
class CarFactory {
public:
    virtual std::unique_ptr<Car> createCar(int power) = 0;
};
```

Factories can accept configuration.

---

## 🔷 Factory Method in Real Systems

### Examples

* UI widgets (Windows / Linux)
* File readers (CSV / JSON / XML)
* Database connections
* CAD feature creation (very Autodesk-relevant)

---

## 🔷 Factory Method and Lifetime Management (C++ Specific)

Always prefer:

```cpp
std::unique_ptr<Product>
```

Avoid:

```cpp
Product*
```

Ownership should be explicit.

---

## 🔷 When to Use Factory Method

✔️ Object creation is complex
✔️ Type decided at runtime
✔️ Want to isolate construction
✔️ Want extensibility

---

## 🔷 When NOT to Use It

❌ Simple object creation
❌ No variation in products
❌ Overengineering small systems

---

## 🔥 Interview One-Liner (MEMORIZE)

> “The Factory Method pattern defines an interface for object creation while allowing subclasses to decide which concrete class to instantiate.”

---

