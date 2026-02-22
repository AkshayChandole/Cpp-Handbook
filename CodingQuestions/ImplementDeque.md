# [Deque implementation](#deque-implementation)

A real `std::deque` is complex (block-based segmented memory).
For interview clarity, I’ll implement a **double-ended queue using a circular dynamic array**.

This gives:

* push_front → O(1) amortized
* push_back → O(1) amortized
* pop_front → O(1)
* pop_back → O(1)
* No shifting
* Circular buffer logic

This is a practical, production-acceptable design for interviews.

---

# 🔹 Custom Implementation of Deque (Circular Buffer Based)

```cpp
#include <iostream>
#include <cassert>
#include <utility>

template <typename T>
class Deque {
private:
    T* data_;
    size_t size_;
    size_t capacity_;
    size_t front_;   // index of first element

    // Get physical index from logical index
    size_t index(size_t i) const {
        return (front_ + i) % capacity_;
    }

    void reallocate(size_t newCapacity) {
        T* newData = new T[newCapacity];

        for (size_t i = 0; i < size_; ++i) {
            newData[i] = std::move(data_[index(i)]);
        }

        delete[] data_;

        data_ = newData;
        capacity_ = newCapacity;
        front_ = 0;
    }

public:
    // 1️⃣ Default constructor
    Deque() : data_(nullptr), size_(0), capacity_(0), front_(0) {}

    // 2️⃣ Destructor
    ~Deque() {
        delete[] data_;
    }

    // 3️⃣ push_back
    void push_back(const T& value) {
        if (size_ == capacity_) {
            size_t newCap = (capacity_ == 0) ? 1 : capacity_ * 2;
            reallocate(newCap);
        }

        size_t rear = index(size_);
        data_[rear] = value;
        ++size_;
    }

    // 4️⃣ push_front
    void push_front(const T& value) {
        if (size_ == capacity_) {
            size_t newCap = (capacity_ == 0) ? 1 : capacity_ * 2;
            reallocate(newCap);
        }

        front_ = (front_ == 0) ? capacity_ - 1 : front_ - 1;
        data_[front_] = value;
        ++size_;
    }

    // 5️⃣ pop_back
    void pop_back() {
        assert(size_ > 0 && "Deque underflow");
        --size_;
    }

    // 6️⃣ pop_front
    void pop_front() {
        assert(size_ > 0 && "Deque underflow");
        front_ = (front_ + 1) % capacity_;
        --size_;
    }

    // 7️⃣ front
    T& front() {
        assert(size_ > 0);
        return data_[front_];
    }

    const T& front() const {
        assert(size_ > 0);
        return data_[front_];
    }

    // 8️⃣ back
    T& back() {
        assert(size_ > 0);
        return data_[index(size_ - 1)];
    }

    const T& back() const {
        assert(size_ > 0);
        return data_[index(size_ - 1)];
    }

    // 9️⃣ operator[]
    T& operator[](size_t i) {
        assert(i < size_);
        return data_[index(i)];
    }

    const T& operator[](size_t i) const {
        assert(i < size_);
        return data_[index(i)];
    }

    // 🔟 size
    size_t size() const { return size_; }

    // 1️⃣1️⃣ empty
    bool empty() const { return size_ == 0; }
};
```

---

# 🔹 main() – Simulation

```cpp
int main() {

    Deque<int> dq;

    // push_back
    dq.push_back(10);
    dq.push_back(20);

    // push_front
    dq.push_front(5);
    dq.push_front(1);

    std::cout << "Front: " << dq.front() << "\n";  // 1
    std::cout << "Back: " << dq.back() << "\n";    // 20

    std::cout << "Elements: ";
    for (size_t i = 0; i < dq.size(); ++i)
        std::cout << dq[i] << " ";
    std::cout << "\n";

    dq.pop_front();
    dq.pop_back();

    std::cout << "After pops:\n";
    std::cout << "Front: " << dq.front() << "\n";  // 5
    std::cout << "Back: " << dq.back() << "\n";    // 10

    return 0;
}
```

---

# 🔹 Output

```
Front: 1
Back: 20
Elements: 1 5 10 20
After pops:
Front: 5
Back: 10
```

---

# 🔹 How It Works Internally

We maintain:

```cpp
T* data_;
size_t size_;
size_t capacity_;
size_t front_;
```

Logical order:

```
front_ → element 0
(front_ + 1) % capacity_ → element 1
(front_ + 2) % capacity_ → element 2
...
```

This allows:

* Insertion at front by decrementing `front_`
* Insertion at back using offset
* No shifting of elements

---

# 🔹 Time Complexity

| Operation  | Complexity     |
| ---------- | -------------- |
| push_front | O(1) amortized |
| push_back  | O(1) amortized |
| pop_front  | O(1)           |
| pop_back   | O(1)           |
| operator[] | O(1)           |

---

# 🔹 How Real std::deque Works (Advanced Insight)

Real `std::deque`:

* Does NOT use single contiguous buffer
* Uses segmented memory blocks
* Keeps map of pointers to blocks
* Avoids large reallocations
* Provides stable push_front/back without moving all elements

This avoids full reallocation when growing at front.

---

# 🔹 Interview-Level Explanation

If interviewer asks:

> Explain your deque design.

You say:

> I implemented deque using a circular dynamic array, allowing efficient insertion and deletion from both ends without shifting elements. The front index moves backward or forward using modulo arithmetic. When full, the buffer doubles in size to maintain amortized constant time operations.

---
