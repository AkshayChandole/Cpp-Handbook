
# [Adapter Pattern](#adapter-pattern)

## 📌 Intent (Official Definition)

> **Convert the interface of a class into another interface that clients expect.
> Adapter lets classes work together that could not otherwise because of incompatible interfaces.**

### In simple words:

> **Adapter is a translator between two incompatible interfaces.**

---

## 🧠 Why Adapter Pattern Exists

In real systems, you often face this situation:

* You already have **working code**
* You integrate:

  * Legacy systems
  * Third-party libraries
  * Old APIs
* Their interface **does not match** what your code expects
* You **cannot modify** the existing code

❌ Rewriting old code is risky

❌ Modifying third-party code is impossible

➡️ **Adapter solves this by translating interfaces without changing either side**

---

## 🔌 Real-World Analogy (Interview Friendly)

* Mobile charger + wall socket
* USB-C → USB-A dongle
* Language translator

👉 The adapter **does not change** the phone or the socket — it just **makes them compatible**.

---

## 🧩 Participants in Adapter Pattern

| Role        | Responsibility                   |
| ----------- | -------------------------------- |
| **Client**  | Uses the expected interface      |
| **Target**  | Interface client expects         |
| **Adaptee** | Existing incompatible class      |
| **Adapter** | Converts Target calls to Adaptee |

---

## ❌ Problem Without Adapter

```cpp
class LegacyPrinter {
public:
    void oldPrint(const std::string& text) {
        std::cout << "Legacy printing: " << text << "\n";
    }
};

class Client {
public:
    void print(const std::string& text) {
        std::cout << "Client printing: " << text << "\n";
    }
};
```

### Problem:

* Client expects `print()`
* Legacy system provides `oldPrint()`
* Interfaces don’t match ❌

---

## ✅ Adapter Pattern Solution

---

## 1️⃣ Target Interface (What client expects)

```cpp
class Printer {
public:
    virtual void print(const std::string& text) = 0;
    virtual ~Printer() = default;
};
```

---

## 2️⃣ Adaptee (Existing incompatible class)

```cpp
class LegacyPrinter {
public:
    void oldPrint(const std::string& text) {
        std::cout << "Legacy printing: " << text << "\n";
    }
};
```

---

## 3️⃣ Adapter (Translation layer)

```cpp
class PrinterAdapter : public Printer {
    LegacyPrinter& legacy;

public:
    PrinterAdapter(LegacyPrinter& lp) : legacy(lp) {}

    void print(const std::string& text) override {
        legacy.oldPrint(text);  // translate call
    }
};
```

---

## 4️⃣ Client Code

```cpp
int main() {
    LegacyPrinter legacy;
    PrinterAdapter adapter(legacy);

    Printer* printer = &adapter;
    printer->print("Hello Adapter");
}
```

```cpp
// Output:
// Legacy printing: Hello Adapter
```

---

## 🔥 Why This Design Works

✔️ Client depends only on **Target interface**

✔️ Legacy code remains unchanged

✔️ Adapter handles translation

✔️ Loose coupling

---

## 🔷 Object Adapter vs Class Adapter (IMPORTANT)

### ✅ Object Adapter (Preferred)

* Uses **composition**
* Adapter **has a** reference to adaptee
* Flexible and safe

```cpp
class Adapter : public Target {
    Adaptee& adaptee;
};
```

✔️ Works with `final` classes

✔️ No multiple inheritance issues

---

### ⚠️ Class Adapter (Less common)

* Uses **multiple inheritance**

```cpp
class Adapter : public Target, private Adaptee {
};
```

❌ Tight coupling

❌ Requires inheritance

❌ Doesn’t work if adaptee is `final`

👉 In modern C++, **object adapter is preferred**

---

## 🔷 Adapter vs Wrapper (Interview Trap)

| Concept | Purpose                       |
| ------- | ----------------------------- |
| Adapter | Changes interface             |
| Wrapper | Adds behavior, same interface |

Adapter focuses on **compatibility**, not enhancement.

---

## 🔷 Adapter with Third-Party Library (Realistic)

```cpp
// Third-party API
class ThirdPartyLogger {
public:
    void logMessage(const char* msg) {
        std::cout << "Third-party log: " << msg << "\n";
    }
};

// Target
class Logger {
public:
    virtual void log(const std::string& msg) = 0;
};

// Adapter
class LoggerAdapter : public Logger {
    ThirdPartyLogger logger;

public:
    void log(const std::string& msg) override {
        logger.logMessage(msg.c_str());
    }
};
```

---

## 🔷 Adapter Using `std::function` (Modern C++)

```cpp
class Adapter {
    std::function<void(const std::string&)> func;

public:
    Adapter(std::function<void(const std::string&)> f) : func(f) {}

    void print(const std::string& s) {
        func(s);
    }
};
```

✔️ No inheritance

✔️ Lightweight

❌ Less explicit design intent

---

## 🔷 When to Use Adapter Pattern

✔️ Integrating legacy systems

✔️ Using third-party APIs

✔️ Migrating old interfaces

✔️ Interface mismatch

---

## 🔷 When NOT to Use Adapter

❌ When you control both interfaces

❌ When redesign is simpler

❌ To add new functionality (use Decorator)

---

## 🔥 Interview One-Liner (MEMORIZE)

> “The Adapter pattern converts the interface of a class into another interface that the client expects, allowing incompatible classes to work together.”

---
