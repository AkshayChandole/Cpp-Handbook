#[How is std::string implemented?](#How-is-std::string-implemented)

## 🔹 std::string – Internal Implementation

### (Focus: Small String Optimization – SSO)

`std::string` is typically a specialization of:

```cpp
std::basic_string<char>
```

Conceptually, it is:

> ✅ A dynamic character array
> ✅ With size & capacity tracking
> ✅ + Small String Optimization (SSO) to avoid heap allocation for short strings

---

# 1️⃣ Basic (Non-SSO) Representation

Without optimization, string would look similar to vector:

```cpp
class String {
    char* data_;
    size_t size_;
    size_t capacity_;
};
```

Memory layout:

```
[data_ ---> heap memory]
```

For:

```cpp
std::string s = "hello";
```

Heap allocation would normally happen.

But this is inefficient for small strings.

---

# 2️⃣ Problem: Too Many Heap Allocations

Most strings in real programs are small:

* filenames
* keys
* error messages
* small JSON fields

Allocating heap memory for every small string:

❌ Expensive
❌ Cache unfriendly
❌ Causes fragmentation

So STL uses **Small String Optimization (SSO)**.

---

# 3️⃣ Small String Optimization (SSO)

Instead of always allocating on heap,
`std::string` stores small strings **inside the object itself**.

Typical implementation (conceptual):

```cpp
class String {
    union {
        struct {
            char* data_;
            size_t size_;
            size_t capacity_;
        } heap;

        struct {
            char buffer[16];
        } sso;
    };

    size_t size_;
};
```

But real layout is more compact.

---

# 4️⃣ Typical Memory Layout (libstdc++ Example)

On 64-bit systems, `std::string` is usually **32 bytes**.

Example layout:

```
| pointer (8B) | size (8B) | capacity (8B) | buffer (16B) |
```

Or optimized via union.

Common SSO capacity:

```
15 characters + '\0'
```

Because:

```cpp
char buffer[16]; // 15 usable + null terminator
```

---

# 5️⃣ How It Decides SSO vs Heap

When constructing:

```cpp
std::string s = "hello";
```

If:

```
length <= SSO_THRESHOLD
```

Then:

* Store inside internal buffer
* No heap allocation

If:

```
length > SSO_THRESHOLD
```

Then:

* Allocate on heap
* Store pointer

---

# 6️⃣ Internal Flagging Mechanism

How does implementation know whether string is SSO or heap?

Different libraries use different tricks:

### 🔹 libstdc++

Uses capacity value to detect SSO.

### 🔹 libc++

Uses last byte to store size for SSO case.

Conceptual idea:

```cpp
if (capacity_ < SSO_THRESHOLD)
    // SSO mode
else
    // Heap mode
```

---

# 7️⃣ Transition from SSO → Heap

Example:

```cpp
std::string s = "hello";   // SSO
s += " world this is long";
```

Steps:

1. Detect overflow beyond SSO buffer
2. Allocate heap memory
3. Copy existing characters
4. Update pointer
5. Switch mode

---

# 8️⃣ push_back() Internals

If SSO and space available:

```cpp
buffer[size_] = ch;
size_++;
buffer[size_] = '\0';
```

If SSO full:

* Allocate heap
* Copy
* Switch to heap mode

If heap mode:

* Reallocate if needed (like vector)

---

# 9️⃣ Why std::string Is Not Exactly std::vector<char>

Key differences:

| Feature         | vector<char> | string |
| --------------- | ------------ | ------ |
| Null terminator | No           | Yes    |
| SSO             | No           | Yes    |
| Char traits     | No           | Yes    |
| C-style interop | Limited      | Direct |

`std::string` guarantees:

```cpp
data()[size()] == '\0'
```

Always null-terminated.

---

# 🔟 Growth Strategy

Similar to vector:

```
new_capacity = old_capacity * 1.5 or 2
```

Amortized O(1) append.

---

# 1️⃣1️⃣ Exception Safety

During reallocation:

* Allocate new memory
* Copy/move characters
* Only update pointer after success

Provides strong exception guarantee.

---

# 1️⃣2️⃣ Move Semantics (C++11+)

When moving:

```cpp
std::string a = "long string...";
std::string b = std::move(a);
```

If heap mode:

* Transfer pointer
* No copy

If SSO mode:

* Just copy buffer (small anyway)

---

# 1️⃣3️⃣ Minimal Educational SSO Implementation

```cpp
#include <cstring>
#include <iostream>

class MyString {
    static const size_t SSO_SIZE = 15;

    size_t size_;
    size_t capacity_;
    char* data_;
    char sso_[SSO_SIZE + 1];

    bool is_sso() const {
        return data_ == sso_;
    }

public:
    MyString(const char* str) {
        size_ = std::strlen(str);

        if (size_ <= SSO_SIZE) {
            data_ = sso_;
            capacity_ = SSO_SIZE;
        } else {
            capacity_ = size_;
            data_ = new char[capacity_ + 1];
        }

        std::memcpy(data_, str, size_ + 1);
    }

    ~MyString() {
        if (!is_sso())
            delete[] data_;
    }
};
```

Real STL implementation is far more optimized and uses unions.

---

# 1️⃣4️⃣ Why SSO Is Extremely Important

Performance gain:

* Avoid heap allocation for small strings
* Better cache locality
* Faster construction/destruction
* Huge improvement in real-world programs

---
