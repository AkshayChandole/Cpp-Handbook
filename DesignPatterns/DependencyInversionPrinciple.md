# [Dependency Inversion Principle (DIP)](#dependency-inversion-principle-dip)

## 📌 Formal Definition

> **High-level modules should not depend on low-level modules.
> Both should depend on abstractions.**

> **Abstractions should not depend on details.
> Details should depend on abstractions.**

---

## 🧠 What does this REALLY mean?

DIP is about **direction of dependency**.

### ❌ Bad dependency direction

```
High-level policy
     ↓
Low-level implementation
```

### ✅ Correct dependency direction

```
High-level policy
     ↓
 Abstraction
     ↑
Low-level implementation
```

---

## 🔥 What is a “High-level” vs “Low-level” Module?

### High-level module

* Contains **business rules**
* Defines **what** the system does

### Low-level module

* Handles **technical details**
* Defines **how** things are done
  (DB, file system, network, hardware)

---

## ❌ Classic DIP Violation

### BAD DESIGN

```cpp
#include <iostream>

class MySQLDatabase {
public:
    void save(const std::string& data) {
        std::cout << "Saving to MySQL\n";
    }
};

class UserService {
    MySQLDatabase db;   // ❌ concrete dependency

public:
    void createUser(const std::string& name) {
        db.save(name);
    }
};
```

---

## ❌ Why This Violates DIP

* `UserService` (business logic) depends on `MySQLDatabase`
* If database changes → `UserService` must change
* Testing becomes hard
* Tight coupling

➡️ **High-level code depends on low-level details**

---

## ✅ DIP-Compliant Design (Correct)

### Step 1: Introduce abstraction

```cpp
class Database {
public:
    virtual void save(const std::string& data) = 0;
    virtual ~Database() = default;
};
```

---

### Step 2: Low-level modules depend on abstraction

```cpp
class MySQLDatabase : public Database {
public:
    void save(const std::string& data) override {
        std::cout << "Saving to MySQL\n";
    }
};

class MongoDatabase : public Database {
public:
    void save(const std::string& data) override {
        std::cout << "Saving to MongoDB\n";
    }
};
```

---

### Step 3: High-level module depends on abstraction

```cpp
class UserService {
    Database& db;   // abstraction dependency

public:
    UserService(Database& database) : db(database) {}

    void createUser(const std::string& name) {
        db.save(name);
    }
};
```

---

### Usage

```cpp
int main() {
    MySQLDatabase mysql;
    UserService service(mysql);

    service.createUser("Akshay");
}
```

```cpp
// Output:
// Saving to MySQL
```

---

## 🔥 Why This Follows DIP

✔️ Business logic depends only on abstraction
✔️ Database can change without touching `UserService`
✔️ Easy to test with mocks
✔️ Low-level details are replaceable

---

## 🔷 Dependency Injection (DIP in Action)

DIP is usually implemented via **Dependency Injection (DI)**.

### 3 Types of DI

---

### 1️⃣ Constructor Injection (BEST)

```cpp
UserService(Database& db) : db(db) {}
```

✔️ Mandatory dependency
✔️ Clear ownership
✔️ Preferred in C++

---

### 2️⃣ Setter Injection

```cpp
void setDatabase(Database& db) {
    this->db = &db;
}
```

⚠️ Dependency can be missing
⚠️ Must validate before use

---

### 3️⃣ Method Injection

```cpp
void createUser(Database& db, const std::string& name);
```

✔️ Temporary dependency
✔️ Good for short-lived usage

---

## 🔷 DIP and Testing (CRITICAL)

### Without DIP (hard to test)

```cpp
UserService service; // tightly coupled to DB
```

---

### With DIP (easy to test)

```cpp
class MockDatabase : public Database {
public:
    void save(const std::string&) override {
        std::cout << "Mock save\n";
    }
};
```

```cpp
MockDatabase mock;
UserService service(mock);
```

✔️ No real DB
✔️ Fast tests
✔️ Deterministic behavior

---

## 🔷 DIP and C++ Headers (Important Detail)

DIP reduces:

* Header coupling
* Recompilation
* Build times

High-level modules include **interfaces only**, not concrete headers.

---

## 🔷 DIP with Smart Pointers

```cpp
class UserService {
    std::shared_ptr<Database> db;
};
```

Use when:

* Shared ownership
* Runtime lifetime management needed

Prefer references when ownership is external.

---

## 🔷 DIP Without Inheritance (Modern C++)

### Using `std::function`

```cpp
class UserService {
    std::function<void(const std::string&)> saveFn;

public:
    UserService(std::function<void(const std::string&)> fn)
        : saveFn(fn) {}

    void createUser(const std::string& name) {
        saveFn(name);
    }
};
```

✔️ Still DIP
✔️ No inheritance
✔️ Highly flexible

---

## 🔥 How to Identify DIP Violations (INTERVIEW TOOL)

Ask:

1. Does business logic include concrete headers?
2. Does changing infrastructure force business logic changes?
3. Can I mock dependencies easily?
4. Do constructors accept concrete types?

If YES → DIP violation.

---

## 🔥 DIP Rule You Can Memorize

> **High-level code defines interfaces.
> Low-level code implements them.**

---

## 🔥 Interview One-Liner (MEMORIZE)

> “The Dependency Inversion Principle states that high-level modules should depend on abstractions rather than concrete implementations, allowing details to vary independently.”

---


