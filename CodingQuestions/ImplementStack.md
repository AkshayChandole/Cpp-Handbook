# [Stack Implementation](#stack-implementation)

Below is a **clean implementation of a generic Stack in C++**, backed by a dynamic array (like how `std::stack` uses a container internally).

This version supports:

* push
* pop
* top
* size
* empty
* dynamic resizing
* Rule of Five
* exception safety for basic operations

---

# 🔹 Custom Implementation of Stack (Dynamic Array Based)

```cpp
#include <iostream>
#include <cassert>
#include <utility>

template <typename T>
class Stack {
private:
    T* data_;
    size_t size_;
    size_t capacity_;

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
    Stack() : data_(nullptr), size_(0), capacity_(0) {}

    // 2️⃣ Destructor
    ~Stack() {
        delete[] data_;
    }

    // 3️⃣ Copy constructor
    Stack(const Stack& other)
        : data_(new T[other.capacity_]),
          size_(other.size_),
          capacity_(other.capacity_) {

        for (size_t i = 0; i < size_; ++i) {
            data_[i] = other.data_[i];
        }
    }

    // 4️⃣ Copy assignment
    Stack& operator=(const Stack& other) {
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
    Stack(Stack&& other) noexcept
        : data_(other.data_),
          size_(other.size_),
          capacity_(other.capacity_) {

        other.data_ = nullptr;
        other.size_ = 0;
        other.capacity_ = 0;
    }

    // 6️⃣ Move assignment
    Stack& operator=(Stack&& other) noexcept {
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

    // 7️⃣ push
    void push(const T& value) {
        if (size_ == capacity_) {
            size_t newCapacity = (capacity_ == 0) ? 1 : capacity_ * 2;
            reallocate(newCapacity);
        }

        data_[size_++] = value;
    }

    // 8️⃣ pop
    void pop() {
        assert(size_ > 0 && "Stack underflow");
        --size_;
    }

    // 9️⃣ top
    T& top() {
        assert(size_ > 0 && "Stack is empty");
        return data_[size_ - 1];
    }

    const T& top() const {
        assert(size_ > 0 && "Stack is empty");
        return data_[size_ - 1];
    }

    // 🔟 size
    size_t size() const {
        return size_;
    }

    // 1️⃣1️⃣ empty
    bool empty() const {
        return size_ == 0;
    }
};
```

---

# 🔹 main() – Simulation

```cpp
int main() {

    Stack<int> s;

    // 1️⃣ Push elements
    s.push(10);
    s.push(20);
    s.push(30);

    std::cout << "Top: " << s.top() << "\n";   // 30
    std::cout << "Size: " << s.size() << "\n";

    // 2️⃣ Pop
    s.pop();
    std::cout << "Top after pop: " << s.top() << "\n";  // 20

    // 3️⃣ Copy stack
    Stack<int> s2 = s;
    std::cout << "Copied stack top: " << s2.top() << "\n";

    // 4️⃣ Move stack
    Stack<int> s3 = std::move(s2);
    std::cout << "Moved stack top: " << s3.top() << "\n";

    return 0;
}
```

---

# 🔹 Output

```
Top: 30
Size: 3
Top after pop: 20
Copied stack top: 20
Moved stack top: 20
```

---

# 🔹 How It Works Internally

Stack maintains:

```cpp
T* data_;
size_t size_;
size_t capacity_;
```

* `size_` → number of elements in stack
* `capacity_` → allocated memory
* push → add at end
* pop → decrement size
* top → access last element

It follows LIFO (Last In First Out).

---

# 🔹 Time Complexity

| Operation | Complexity     |
| --------- | -------------- |
| push      | O(1) amortized |
| pop       | O(1)           |
| top       | O(1)           |
| resize    | O(n)           |

---

# 🔹 Interview-Level Explanation

If interviewer asks:

> Explain your stack implementation.

Answer:

> I implemented stack using a dynamically resizing array. push inserts at the end and doubles capacity when full to ensure amortized constant-time insertion. pop simply decrements size. top returns the last element. I implemented Rule of Five to manage memory safely and used move semantics for efficient transfers.

---

# 🔹 What Production std::stack Does

`std::stack` is actually:

```cpp
template<class T, class Container = std::deque<T>>
class stack;
```

It is an **adapter**, not a container itself.

Default underlying container → `std::deque`

---
