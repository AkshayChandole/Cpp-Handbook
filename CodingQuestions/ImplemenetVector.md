 # [Vector implementation](#vector-implementation)

Below is a **clean, production-style simplified implementation of `std::vector`**.

This implementation supports:

* Dynamic resizing
* push_back
* pop_back
* operator[]
* size / capacity
* reserve
* copy constructor
* copy assignment
* move constructor
* move assignment
* destructor
* reallocation logic

⚠️ Simplified version:

* Uses `new/delete`
* Does not use allocator
* Not fully exception-safe like STL
* Does not implement iterators

---

# 🔹 Custom Implementation of Vector

```cpp
#include <iostream>
#include <utility>
#include <cassert>

template <typename T>
class Vector {
private:
    T* data_;
    size_t size_;
    size_t capacity_;

    // Reallocate memory
    void reallocate(size_t newCapacity) {
        T* newData = new T[newCapacity];

        for (size_t i = 0; i < size_; ++i) {
            newData[i] = std::move(data_[i]);
        }

        delete[] data_;
        data_ = newData;
        capacity_ = newCapacity;
    }

public:
    // 1️⃣ Default constructor
    Vector() : data_(nullptr), size_(0), capacity_(0) {}

    // 2️⃣ Destructor
    ~Vector() {
        delete[] data_;
    }

    // 3️⃣ Copy constructor
    Vector(const Vector& other)
        : data_(new T[other.capacity_]),
          size_(other.size_),
          capacity_(other.capacity_) {

        for (size_t i = 0; i < size_; ++i) {
            data_[i] = other.data_[i];
        }
    }

    // 4️⃣ Copy assignment
    Vector& operator=(const Vector& other) {
        if (this != &other) {
            delete[] data_;

            data_ = new T[other.capacity_];
            size_ = other.size_;
            capacity_ = other.capacity_;

            for (size_t i = 0; i < size_; ++i) {
                data_[i] = other.data_[i];
            }
        }
        return *this;
    }

    // 5️⃣ Move constructor
    Vector(Vector&& other) noexcept
        : data_(other.data_),
          size_(other.size_),
          capacity_(other.capacity_) {

        other.data_ = nullptr;
        other.size_ = 0;
        other.capacity_ = 0;
    }

    // 6️⃣ Move assignment
    Vector& operator=(Vector&& other) noexcept {
        if (this != &other) {
            delete[] data_;

            data_ = other.data_;
            size_ = other.size_;
            capacity_ = other.capacity_;

            other.data_ = nullptr;
            other.size_ = 0;
            other.capacity_ = 0;
        }
        return *this;
    }

    // 7️⃣ push_back
    void push_back(const T& value) {
        if (size_ == capacity_) {
            size_t newCapacity = capacity_ == 0 ? 1 : capacity_ * 2;
            reallocate(newCapacity);
        }

        data_[size_] = value;
        size_++;
    }

    // 8️⃣ pop_back
    void pop_back() {
        if (size_ > 0) {
            size_--;
        }
    }

    // 9️⃣ operator[]
    T& operator[](size_t index) {
        assert(index < size_ && "Index out of bounds");
        return data_[index];
    }

    const T& operator[](size_t index) const {
        assert(index < size_ && "Index out of bounds");
        return data_[index];
    }

    // 🔟 size
    size_t size() const {
        return size_;
    }

    // 1️⃣1️⃣ capacity
    size_t capacity() const {
        return capacity_;
    }

    // 1️⃣2️⃣ reserve
    void reserve(size_t newCapacity) {
        if (newCapacity > capacity_) {
            reallocate(newCapacity);
        }
    }

    // 1️⃣3️⃣ empty
    bool empty() const {
        return size_ == 0;
    }
};
```

---


# 🔹 main() – Simulation

```cpp
int main() {

    Vector<int> vec;

    // 1️⃣ push_back
    for (int i = 1; i <= 5; ++i) {
        vec.push_back(i * 10);
        std::cout << "Added: " << i * 10
                  << " | Size: " << vec.size()
                  << " | Capacity: " << vec.capacity()
                  << "\n";
    }

    // 2️⃣ Access elements
    std::cout << "Elements:\n";
    for (size_t i = 0; i < vec.size(); ++i) {
        std::cout << vec[i] << " ";
    }
    std::cout << "\n";

    // 3️⃣ pop_back
    vec.pop_back();
    std::cout << "After pop_back, Size: "
              << vec.size() << "\n";

    // 4️⃣ Copy constructor
    Vector<int> vec2 = vec;
    std::cout << "Copied vector elements:\n";
    for (size_t i = 0; i < vec2.size(); ++i) {
        std::cout << vec2[i] << " ";
    }

    return 0;
}
```

---

# 🔹 How It Works Internally

Vector maintains:

```cpp
T* data_;
size_t size_;
size_t capacity_;
```

* `size_` → number of valid elements
* `capacity_` → allocated memory size
* When size == capacity → double capacity

---

# 🔹 Time Complexity

| Operation  | Complexity     |
| ---------- | -------------- |
| push_back  | O(1) amortized |
| pop_back   | O(1)           |
| operator[] | O(1)           |
| reserve    | O(n)           |
| reallocate | O(n)           |

---

# 🔹 Interview-Level Explanation

If interviewer asks:

> Explain your vector implementation.

Answer:

> I implemented a dynamic array that maintains size and capacity. When the container becomes full, it reallocates memory with doubled capacity to maintain amortized constant-time insertion. I implemented Rule of Five to manage ownership correctly and used move semantics during reallocation for efficiency.

---

# 🔹 What This Implementation Is Missing

Production `std::vector` also includes:

* Allocator support
* Placement new
* Explicit destructor calls
* Strong exception safety guarantees
* Iterators
* emplace_back
* shrink_to_fit
* Insert/erase at arbitrary position

---

