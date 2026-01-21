
# [Single Responsibility Principle (SRP)](#single-responsibility-principle-srp)

## 📌 Definition

> **A class should have only one reason to change.**

This does **NOT** mean:

* One method per class ❌
* Very small classes ❌

It means:

> **A class should have one, and only one, responsibility — one axis of change.**

---

## 🧠 What is a “Responsibility”?

A **responsibility** is **not** a function.

A responsibility is:

* A **reason why the class would need to change**

### Examples of reasons to change:

* Business rules change
* Database schema changes
* File format changes
* Logging mechanism changes
* UI changes

If **more than one of these** affects a class → **SRP violation**

---

## 🔥 Classic SRP Violation Example

### ❌ WRONG DESIGN

```cpp
#include <iostream>
#include <fstream>

class Report {
public:
    std::string content;

    void generateReport() {
        content = "Sales Report Data";
    }

    void saveToFile(const std::string& filename) {
        std::ofstream file(filename);
        file << content;
    }

    void print() {
        std::cout << content;
    }
};
```

---

## ❌ Why This Violates SRP

This class has **multiple reasons to change**:

| Responsibility | Reason to Change        |
| -------------- | ----------------------- |
| Business logic | Report format changes   |
| Persistence    | File system changes     |
| Presentation   | Printing format changes |

➡️ **3 responsibilities = SRP violation**

---

## ✅ SRP-Compliant Design (Correct)

### Step 1: Business logic only

```cpp
class Report {
public:
    std::string generate() const {
        return "Sales Report Data";
    }
};
```

---

### Step 2: Persistence responsibility

```cpp
#include <fstream>

class ReportSaver {
public:
    void saveToFile(const std::string& data,
                    const std::string& filename) {
        std::ofstream file(filename);
        file << data;
    }
};
```

---

### Step 3: Presentation responsibility

```cpp
#include <iostream>

class ReportPrinter {
public:
    void print(const std::string& data) {
        std::cout << data;
    }
};
```

---

### Usage

```cpp
int main() {
    Report report;
    ReportSaver saver;
    ReportPrinter printer;

    auto data = report.generate();
    saver.saveToFile(data, "report.txt");
    printer.print(data);
}
```

---

## 🔷 Why This Design is Better

✔️ Each class has **one reason to change**
✔️ Easy to test
✔️ Easy to extend
✔️ Changes are isolated

---

## 🔥 SRP Is About CHANGE, Not SIZE (INTERVIEW TRAP)

### ❌ Wrong assumption

> “SRP means one method per class”

No.

### ✅ Correct thinking

> “How many different kinds of changes can affect this class?”

---

## 🔷 SRP and High Cohesion

SRP naturally leads to **high cohesion**:

* All methods in a class serve one purpose
* No unrelated logic

---

## 🔷 SRP and Microservices (Real-world mapping)

| Monolith             | SRP |
| -------------------- | --- |
| God service          | ❌   |
| Auth service         | ✅   |
| Billing service      | ✅   |
| Notification service | ✅   |

Same principle, different scale.

---

## 🔷 Common SRP Violations in Real Code

❌ Class doing **logging + business logic**

❌ Model class doing **DB + validation**

❌ Controller doing **logic + persistence**

❌ Utility class doing “everything”

---

## 🔥 SRP in STL Example

### ❌ Bad

```cpp
class FileLogger {
public:
    void log(const std::string&);
    void rotateLogs();
    void compressLogs();
};
```

Multiple responsibilities.

---

### ✅ Good

```cpp
class Logger { /* logging only */ };
class LogRotator { /* rotation only */ };
class LogCompressor { /* compression only */ };
```

---

## 🔥 Interview One-Liner (MEMORIZE)

> “Single Responsibility Principle states that a class should have only one reason to change, meaning it should encapsulate a single responsibility or concern.”


## ✅ Next Step

Shall we move to **Open/Closed Principle (OCP)** next?
I’ll explain it with **polymorphism, inheritance, and C++ examples**, plus **common traps**.
