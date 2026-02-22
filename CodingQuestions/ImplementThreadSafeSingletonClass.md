# [Implement thread safe singleton class](#implement-thread-safe-singleton-class)

There are multiple correct ways to implement a **thread-safe Singleton** in C++.

1. ✅ Recommended Modern C++ (Meyers Singleton)
2. ✅ Double-Checked Locking (manual control)
3. ✅ Advanced: Using std::call_once

---

# ✅ 1️⃣ Recommended: Meyers Singleton (C++11 and above)

This is the **cleanest and safest implementation**.

```cpp
#include <iostream>
#include <mutex>

class Singleton {
private:
    Singleton() {
        std::cout << "Constructor called\n";
    }

    ~Singleton() {
        std::cout << "Destructor called\n";
    }

    // Delete copy operations
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

public:
    static Singleton& getInstance() {
        static Singleton instance;  // Thread-safe in C++11+
        return instance;
    }

    void doSomething() {
        std::cout << "Doing something\n";
    }
};
```

---

## 🔹 Why Is This Thread-Safe?

Since C++11:

> Static local variable initialization is guaranteed to be thread-safe.

Only one thread initializes it.
Other threads wait.

No mutex needed.

---

## 🔹 Usage

```cpp
int main() {
    Singleton& s1 = Singleton::getInstance();
    Singleton& s2 = Singleton::getInstance();

    s1.doSomething();

    std::cout << "Same instance? "
              << (&s1 == &s2) << "\n";

    return 0;
}
```

---

# ✅ 2️⃣ Double-Checked Locking (Manual Version)

Used when you want dynamic allocation.

```cpp
#include <iostream>
#include <mutex>

class Singleton {
private:
    static Singleton* instance_;
    static std::mutex mutex_;

    Singleton() {
        std::cout << "Constructor called\n";
    }

public:
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

    static Singleton* getInstance() {
        if (instance_ == nullptr) {
            std::lock_guard<std::mutex> lock(mutex_);
            if (instance_ == nullptr) {
                instance_ = new Singleton();
            }
        }
        return instance_;
    }

    void doSomething() {
        std::cout << "Working...\n";
    }
};

// Static member definitions
Singleton* Singleton::instance_ = nullptr;
std::mutex Singleton::mutex_;
```

---

## 🔹 Why Double-Checked?

Without outer check:

Every call locks mutex → slow.

With double-check:

* Lock only on first initialization
* Fast afterward

---

# ⚠️ Why Pre-C++11 Singleton Was Broken

Before C++11:

```cpp
static Singleton instance;
```

Was NOT guaranteed thread-safe.

Two threads could initialize simultaneously → undefined behavior.

---

# 🔥 Advanced: Using std::call_once

Alternative safe version:

```cpp
#include <mutex>

class Singleton {
private:
    static Singleton* instance_;
    static std::once_flag initFlag_;

    Singleton() {}

public:
    static Singleton* getInstance() {
        std::call_once(initFlag_, []() {
            instance_ = new Singleton();
        });
        return instance_;
    }
};

Singleton* Singleton::instance_ = nullptr;
std::once_flag Singleton::initFlag_;
```

---

# 🔹 Comparison

| Method                 | Thread-Safe | Recommended | Notes            |
| ---------------------- | ----------- | ----------- | ---------------- |
| Meyers Singleton       | ✅           | ⭐⭐⭐⭐⭐       | Best practice    |
| Double-Checked Locking | ✅           | ⭐⭐⭐         | More complex     |
| call_once              | ✅           | ⭐⭐⭐⭐        | Explicit control |

---

# 🔹 Interview-Level Explanation

If interviewer asks:

> How do you implement thread-safe Singleton?

Answer:

> In modern C++, the preferred way is using a function-local static variable because C++11 guarantees thread-safe initialization. This avoids manual locking and is efficient. For more control over allocation or destruction timing, we can use double-checked locking with std::mutex or std::call_once.

---
