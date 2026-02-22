# [String implementation](#string-implementation)

Below is a **clean, production-style simplified implementation of a custom String class**.

This version supports:

* Dynamic memory management
* Rule of Five
* Copy & Move semantics
* operator[]
* operator+
* operator+=
* size(), c_str()
* Deep copy behavior

⚠️ Simplified version:

* No small string optimization (SSO)
* No allocator support
* Basic exception safety

---

# 🔹 Custom Implementation of String Class

```cpp
#include <iostream>
#include <cstring>
#include <utility>
#include <cassert>

class String {
private:
    char* data_;
    size_t size_;

public:
    // 1️⃣ Default constructor
    String() : data_(new char[1]), size_(0) {
        data_[0] = '\0';
    }

    // 2️⃣ Constructor from C-string
    String(const char* str) {
        if (str) {
            size_ = std::strlen(str);
            data_ = new char[size_ + 1];
            std::memcpy(data_, str, size_ + 1);
        } else {
            size_ = 0;
            data_ = new char[1];
            data_[0] = '\0';
        }
    }

    // 3️⃣ Copy constructor (Deep copy)
    String(const String& other)
        : size_(other.size_) {

        data_ = new char[size_ + 1];
        std::memcpy(data_, other.data_, size_ + 1);
    }

    // 4️⃣ Move constructor
    String(String&& other) noexcept
        : data_(other.data_), size_(other.size_) {

        other.data_ = nullptr;
        other.size_ = 0;
    }

    // 5️⃣ Copy assignment
    String& operator=(const String& other) {
        if (this != &other) {
            char* newData = new char[other.size_ + 1];
            std::memcpy(newData, other.data_, other.size_ + 1);

            delete[] data_;
            data_ = newData;
            size_ = other.size_;
        }
        return *this;
    }

    // 6️⃣ Move assignment
    String& operator=(String&& other) noexcept {
        if (this != &other) {
            delete[] data_;

            data_ = other.data_;
            size_ = other.size_;

            other.data_ = nullptr;
            other.size_ = 0;
        }
        return *this;
    }

    // 7️⃣ Destructor
    ~String() {
        delete[] data_;
    }

    // 8️⃣ operator[]
    char& operator[](size_t index) {
        assert(index < size_);
        return data_[index];
    }

    const char& operator[](size_t index) const {
        assert(index < size_);
        return data_[index];
    }

    // 9️⃣ size()
    size_t size() const {
        return size_;
    }

    // 🔟 c_str()
    const char* c_str() const {
        return data_;
    }

    // 1️⃣1️⃣ operator+=
    String& operator+=(const String& other) {
        size_t newSize = size_ + other.size_;
        char* newData = new char[newSize + 1];

        std::memcpy(newData, data_, size_);
        std::memcpy(newData + size_, other.data_, other.size_ + 1);

        delete[] data_;
        data_ = newData;
        size_ = newSize;

        return *this;
    }

    // 1️⃣2️⃣ operator+
    friend String operator+(const String& lhs, const String& rhs) {
        String temp(lhs);
        temp += rhs;
        return temp;
    }

    // 1️⃣3️⃣ Print helper
    void print() const {
        std::cout << data_ << "\n";
    }
};
```

---

# 🔹 main() – Simulation & Validation

```cpp
int main() {

    // 1️⃣ Basic construction
    String s1("Hello");
    String s2(" World");

    s1.print();
    s2.print();

    // 2️⃣ Copy constructor
    String s3 = s1;
    s3.print();

    // 3️⃣ Move constructor
    String s4 = std::move(s2);
    s4.print();

    // 4️⃣ Concatenation
    String s5 = s1 + s4;
    s5.print();

    // 5️⃣ operator+=
    s1 += String(" C++");
    s1.print();

    // 6️⃣ Index access
    std::cout << "First char: " << s1[0] << "\n";

    return 0;
}
```

---

# 🔹 What This Implementation Demonstrates

### ✅ Deep Copy

Each object owns its own memory.

### ✅ Rule of Five

* Destructor
* Copy constructor
* Copy assignment
* Move constructor
* Move assignment

### ✅ Move Optimization

Avoids unnecessary memory allocation during temporary usage.

---

# 🔹 Time Complexity

| Operation    | Complexity |
| ------------ | ---------- |
| Construction | O(n)       |
| Copy         | O(n)       |
| Move         | O(1)       |
| operator+=   | O(n)       |
| operator+    | O(n)       |

---

# 🔹 Interview-Level Explanation

If interviewer asks:

> Explain your String implementation.

You say:

> I implemented a dynamic character buffer managing memory manually. I ensured deep copy semantics, followed Rule of Five for correct ownership handling, used move semantics for efficiency, and maintained null-termination invariant. Concatenation reallocates memory and copies content. The design maintains strong ownership and prevents memory leaks.

---

# 🔹 What This Implementation Is Missing (Senior Depth)

Real `std::string` includes:

* Small String Optimization (SSO)
* Capacity management
* Allocator support
* Exception safety guarantees
* Short string stored inline (no heap)
* Iterators
* Insert / erase
* find, substr, etc.

---
