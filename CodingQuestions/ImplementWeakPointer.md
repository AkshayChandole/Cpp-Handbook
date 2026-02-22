# [Implement weak pointer](#implement-weak-pointer)

To implement `WeakPtr`, we must first slightly upgrade our `SharedPtr` design to include a proper **control block** containing:

* strong_count
* weak_count
* pointer to managed object

Because weak pointer:

* Does NOT own object
* Must observe control block
* Must allow `lock()` to create shared_ptr safely

---

# 🔹 Design Overview

We implement:

```cpp
SharedPtr<T>
WeakPtr<T>
ControlBlock<T>
```

---

# 🔹 Control Block

```cpp id="cblock1"
template<typename T>
struct ControlBlock {
    T* ptr;
    size_t strong_count;
    size_t weak_count;

    ControlBlock(T* p)
        : ptr(p), strong_count(1), weak_count(0) {}
};
```

---

# 🔹 SharedPtr Implementation (With Weak Support)

```cpp id="sharedptr1"
#include <iostream>
#include <cassert>

template<typename T>
class WeakPtr;

template<typename T>
class SharedPtr {
private:
    ControlBlock<T>* cb_;

    void release() {
        if (!cb_) return;

        cb_->strong_count--;

        if (cb_->strong_count == 0) {
            delete cb_->ptr;
            cb_->ptr = nullptr;

            if (cb_->weak_count == 0) {
                delete cb_;
            }
        }

        cb_ = nullptr;
    }

public:
    SharedPtr() : cb_(nullptr) {}

    explicit SharedPtr(T* ptr) {
        cb_ = new ControlBlock<T>(ptr);
    }

    // Copy constructor
    SharedPtr(const SharedPtr& other) {
        cb_ = other.cb_;
        if (cb_)
            cb_->strong_count++;
    }

    // Constructor from WeakPtr
    SharedPtr(const WeakPtr<T>& weak) {
        cb_ = weak.cb_;
        if (cb_ && cb_->strong_count > 0) {
            cb_->strong_count++;
        } else {
            cb_ = nullptr;
        }
    }

    ~SharedPtr() {
        release();
    }

    SharedPtr& operator=(const SharedPtr& other) {
        if (this != &other) {
            release();
            cb_ = other.cb_;
            if (cb_)
                cb_->strong_count++;
        }
        return *this;
    }

    T* get() const {
        return cb_ ? cb_->ptr : nullptr;
    }

    T& operator*() const {
        return *(cb_->ptr);
    }

    T* operator->() const {
        return cb_->ptr;
    }

    size_t use_count() const {
        return cb_ ? cb_->strong_count : 0;
    }

    friend class WeakPtr<T>;
};
```

---

# 🔹 WeakPtr Implementation

```cpp id="weakptr1"
template<typename T>
class WeakPtr {
private:
    ControlBlock<T>* cb_;

    void release() {
        if (!cb_) return;

        cb_->weak_count--;

        if (cb_->strong_count == 0 && cb_->weak_count == 0) {
            delete cb_;
        }

        cb_ = nullptr;
    }

public:
    WeakPtr() : cb_(nullptr) {}

    // Construct from SharedPtr
    WeakPtr(const SharedPtr<T>& shared) {
        cb_ = shared.cb_;
        if (cb_)
            cb_->weak_count++;
    }

    // Copy constructor
    WeakPtr(const WeakPtr& other) {
        cb_ = other.cb_;
        if (cb_)
            cb_->weak_count++;
    }

    ~WeakPtr() {
        release();
    }

    WeakPtr& operator=(const WeakPtr& other) {
        if (this != &other) {
            release();
            cb_ = other.cb_;
            if (cb_)
                cb_->weak_count++;
        }
        return *this;
    }

    // Check if object still exists
    bool expired() const {
        return !cb_ || cb_->strong_count == 0;
    }

    // Convert to SharedPtr
    SharedPtr<T> lock() const {
        if (!expired()) {
            return SharedPtr<T>(*this);
        }
        return SharedPtr<T>();
    }

    friend class SharedPtr<T>;
};
```

---

# 🔹 Test Class

```cpp id="testclass1"
class Test {
public:
    Test() { std::cout << "Constructor\n"; }
    ~Test() { std::cout << "Destructor\n"; }
};
```

---

# 🔹 main() – Simulation

```cpp id="mainweak1"
int main() {

    WeakPtr<Test> weak;

    {
        SharedPtr<Test> sp(new Test());

        weak = WeakPtr<Test>(sp);

        std::cout << "Strong count: "
                  << sp.use_count() << "\n";

        if (auto locked = weak.lock()) {
            std::cout << "Object is alive\n";
        }
    }

    // sp destroyed here

    if (weak.expired()) {
        std::cout << "Object expired\n";
    }

    return 0;
}
```

---

# 🔹 Expected Output

```
Constructor
Strong count: 1
Object is alive
Destructor
Object expired
```

---

# 🔹 How It Works Internally

Control block holds:

```
strong_count → number of SharedPtr
weak_count   → number of WeakPtr
```

When:

```
strong_count == 0
→ object deleted
```

When:

```
strong_count == 0 AND weak_count == 0
→ control block deleted
```

---

# 🔹 Interview-Level Explanation

If interviewer asks:

> How does weak_ptr work internally?

Answer:

> weak_ptr holds a pointer to the same control block as shared_ptr but does not increment the strong reference count. It increments only the weak count. When strong count drops to zero, the managed object is destroyed, but the control block remains until weak count also reaches zero. lock() checks whether strong count is non-zero before creating a new shared_ptr.

---

# 🔥 Important Note

This implementation is **not thread-safe**.

Production `std::shared_ptr` uses:

```cpp
std::atomic<size_t>
```

for counters.

---
