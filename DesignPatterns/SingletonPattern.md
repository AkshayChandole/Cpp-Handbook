# [Singleton Design Pattern](#singleton-design-pattern)

## 📌 Intent (Canonical Definition)

> **Ensure that a class has only one instance and provide a global point of access to it.**

---

## 🧠 What Problem Does Singleton Solve?

Some resources must have:

* **Exactly one instance**
* **Shared access across the system**

Examples:

* Logger
* Configuration manager
* Cache
* Thread pool
* Device driver interface

Creating multiple instances would cause:

* Inconsistent state
* Resource contention
* Undefined behavior

---

## 🔷 Core Characteristics

A Singleton class must:

1. **Prevent external instantiation**
2. **Control object creation**
3. **Expose a single access point**

---

## 🔥 Basic Structure of Singleton

```cpp
class Singleton {
private:
    Singleton();                  // private constructor
    Singleton(const Singleton&) = delete;            // no copy
    Singleton& operator=(const Singleton&) = delete; // no assign

public:
    static Singleton& getInstance();
};
```

---

## 🔷 Canonical C++11 Singleton (BEST PRACTICE)

### ✅ Meyers’ Singleton (Recommended)

```cpp
class Logger {
public:
    static Logger& getInstance() {
        static Logger instance;  // created once
        return instance;
    }

    void log(const std::string& msg) {
        std::cout << msg << "\n";
    }

private:
    Logger() {}  // private constructor
};
```

---

## 🔥 Why This Is the BEST Implementation

### 1️⃣ Thread-safe (C++11+)

* Static local initialization is **guaranteed thread-safe**

### 2️⃣ Lazy initialization

* Object created only when first accessed

### 3️⃣ No memory leaks

* Destructor called at program termination

### 4️⃣ Simple & clean

* No mutexes
* No pointers

---

## 🔷 Usage

```cpp
int main() {
    Logger::getInstance().log("Hello");
    Logger::getInstance().log("World");
}
```

```cpp
// Output:
// Hello
// World
```

---

## 🔷 Why Constructor Must Be Private

```cpp
Logger l;  // ❌ should not compile
```

Private constructor:

* Prevents `new Logger()`
* Prevents stack allocation
* Enforces controlled creation

---

## 🔷 Prevent Copying and Assignment (CRITICAL)

```cpp
Logger a = Logger::getInstance(); // ❌
Logger b(a);                      // ❌
```

Blocked using:

```cpp
Logger(const Logger&) = delete;
Logger& operator=(const Logger&) = delete;
```

---

## 🔷 Thread Safety: Pre-C++11 Problem

### ❌ Broken Singleton (Old)

```cpp
static Logger* instance = nullptr;

if (!instance)
    instance = new Logger();  // ❌ race condition
```

Two threads can create **two instances**.

---

### ❌ Mutex-Based Singleton (Works but inferior)

```cpp
static std::mutex mtx;

Logger& getInstance() {
    std::lock_guard<std::mutex> lock(mtx);
    static Logger instance;
    return instance;
}
```

✔️ Thread-safe
❌ Slower
❌ Overkill

---

## 🔷 Singleton with Dynamic Lifetime Control

```cpp
class Config {
public:
    static Config& getInstance() {
        static Config instance;
        return instance;
    }

    void load() {}
};
```

This is enough for **99% of cases**.

---

## 🔷 When Singleton Is a BAD Idea (VERY IMPORTANT)

Singleton introduces:

* **Global state**
* **Hidden dependencies**
* **Hard-to-test code**

### ❌ Problems

* Breaks dependency injection
* Tight coupling
* Order-of-destruction issues

---

## 🔥 Example: Singleton Hurting Testability

```cpp
class Service {
public:
    void run() {
        Logger::getInstance().log("Run");
    }
};
```

You cannot easily:

* Mock `Logger`
* Replace behavior for tests

---

## 🔷 Better Alternative (When Possible)

Use **Dependency Injection** instead:

```cpp
class Service {
    Logger& logger;

public:
    Service(Logger& l) : logger(l) {}
};
```

Singleton should be used **sparingly**.

---

## 🔷 Singleton vs Static Class (INTERVIEW TRAP)

| Aspect           | Singleton | Static Class |
| ---------------- | --------- | ------------ |
| Object existence | Yes       | No           |
| State            | Yes       | Limited      |
| Polymorphism     | Yes       | No           |
| Interface        | Yes       | No           |

Singleton is **an object**, not just a namespace.

---

## 🔷 Common Singleton Variants

### 1️⃣ Eager Singleton

* Instance created at program startup
* No lazy loading

### 2️⃣ Lazy Singleton (Meyers)

* Created on first use ✅

### 3️⃣ Thread-local Singleton

```cpp
thread_local static Logger instance;
```

One instance **per thread**.

---


## 🔥 Interview One-Liner (MEMORIZE)

> “The Singleton pattern ensures that a class has only one instance and provides a global access point, typically implemented in modern C++ using a function-local static object.”

---


