# [Implement lock guard](#implement-lock-guard)

## 🔐 `std::lock_guard` in C++

`std::lock_guard` (introduced in **C++11**) is a simple RAII wrapper over a mutex.

👉 It **locks the mutex in constructor**
👉 It **unlocks automatically in destructor**
👉 It is **non-copyable**
👉 It guarantees **exception safety**

Defined in:

```cpp
#include <mutex>
```

---

# 🧠 Basic Idea (RAII)

```cpp
{
    std::lock_guard<std::mutex> lock(mtx);
    // critical section
} // automatically unlocks here
```

Even if exception occurs → mutex gets unlocked.

---

# 🔨 Let’s Implement Our Own `LockGuard`

We’ll create a minimal version similar to `std::lock_guard`.

---

## ✅ Version 1 — Basic Implementation

```cpp
#include <mutex>

template<typename Mutex>
class LockGuard {
private:
    Mutex& mtx;

public:
    // Constructor locks the mutex
    explicit LockGuard(Mutex& m) : mtx(m) {
        mtx.lock();
    }

    // Destructor unlocks the mutex
    ~LockGuard() {
        mtx.unlock();
    }

    // Delete copy constructor
    LockGuard(const LockGuard&) = delete;

    // Delete copy assignment
    LockGuard& operator=(const LockGuard&) = delete;
};
```

---

## 🔎 Why `explicit`?

Prevents accidental conversion:

```cpp
LockGuard<std::mutex> lg = mtx;  // ❌ Not allowed
```

---

## 🧪 Usage Example

```cpp
#include <iostream>
#include <thread>
#include <vector>
#include <mutex>

std::mutex mtx;
int counter = 0;

void increment() {
    LockGuard<std::mutex> lock(mtx);
    ++counter;
}

int main() {
    std::vector<std::thread> threads;

    for(int i = 0; i < 1000; ++i) {
        threads.emplace_back(increment);
    }

    for(auto& t : threads) {
        t.join();
    }

    std::cout << "Counter: " << counter << std::endl;
}
```

Without lock → race condition
With LockGuard → thread-safe

---

# ⚡ Production-Level Improvements

The real `std::lock_guard` supports:

```cpp
std::adopt_lock_t
```

Which allows adopting an already locked mutex.

---

## ✅ Version 2 — Supporting `adopt_lock`

```cpp
#include <mutex>

struct AdoptLockTag {};
constexpr AdoptLockTag adopt_lock{};

template<typename Mutex>
class LockGuard {
private:
    Mutex& mtx;

public:
    // Normal locking constructor
    explicit LockGuard(Mutex& m) : mtx(m) {
        mtx.lock();
    }

    // Adopt lock constructor
    LockGuard(Mutex& m, AdoptLockTag) : mtx(m) {
        // Assume mutex is already locked
    }

    ~LockGuard() {
        mtx.unlock();
    }

    LockGuard(const LockGuard&) = delete;
    LockGuard& operator=(const LockGuard&) = delete;
};
```

Usage:

```cpp
mtx.lock();
LockGuard<std::mutex> lock(mtx, adopt_lock);
```

---

# 🏗 Internal Behavior (What Happens)

1. Constructor → `mutex.lock()`
2. Destructor → `mutex.unlock()`
3. Scope exit → destructor runs
4. Stack unwinding → safe unlock

This is pure RAII pattern.

---

# 🚫 Why No Move Constructor?

`lock_guard` is intentionally non-movable.

Because:

* Ownership must not transfer
* Very lightweight
* Simpler design than `unique_lock`

---

# 🆚 lock_guard vs unique_lock

| Feature         | lock_guard              | unique_lock      |
| --------------- | ----------------------- | ---------------- |
| Lightweight     | ✅                       | ❌                |
| Movable         | ❌                       | ✅                |
| Deferred lock   | ❌                       | ✅                |
| Unlock manually | ❌                       | ✅                |
| Best for        | Simple critical section | Advanced locking |

---

# 🧠 Interview-Level Explanation

If interviewer asks:

> Implement lock_guard

You must mention:

* RAII
* Non-copyable
* Locks in constructor
* Unlocks in destructor
* Supports adopt_lock
* Exception safety
* Why no move support

---

# 🎯 One-Liner Definition

`lock_guard` is a scoped RAII mutex wrapper that provides basic exception-safe mutual exclusion.

---
