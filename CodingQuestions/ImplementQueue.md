# [Queue Implementation](#queue-implementation)

We’ll implement queue using a **circular dynamic array**, because:

* `push` → O(1)
* `pop` → O(1)
* No shifting elements
* More efficient than naive array implementation

This is closer to how real containers behave.

---

# 🔹 Custom Implementation of Queue (Circular Buffer Based)

```cpp
#include <iostream>
#include <cassert>
#include <utility>

template <typename T>
class Queue {
private:
    T* data_;
    size_t size_;
    size_t capacity_;
    size_t front_;
    size_t rear_;

    void reallocate(size_t newCapacity) {
        T* newData = new T[newCapacity];

        for (size_t i = 0; i < size_; ++i) {
            newData[i] = std::move(data_[(front_ + i) % capacity_]);
        }

        delete[] data_;

        data_ = newData;
        capacity_ = newCapacity;
        front_ = 0;
        rear_ = size_;
    }

public:
    // 1️⃣ Default constructor
    Queue() : data_(nullptr), size_(0), capacity_(0), front_(0), rear_(0) {}

    // 2️⃣ Destructor
    ~Queue() {
        delete[] data_;
    }

    // 3️⃣ Copy constructor
    Queue(const Queue& other)
        : data_(new T[other.capacity_]),
          size_(other.size_),
          capacity_(other.capacity_),
          front_(0),
          rear_(other.size_) {

        for (size_t i = 0; i < size_; ++i) {
            data_[i] = other.data_[(other.front_ + i) % other.capacity_];
        }
    }

    // 4️⃣ Copy assignment
    Queue& operator=(const Queue& other) {
        if (this != &other) {
            delete[] data_;

            data_ = new T[other.capacity_];
            size_ = other.size_;
            capacity_ = other.capacity_;
            front_ = 0;
            rear_ = size_;

            for (size_t i = 0; i < size_; ++i) {
                data_[i] = other.data_[(other.front_ + i) % other.capacity_];
            }
        }
        return *this;
    }

    // 5️⃣ Move constructor
    Queue(Queue&& other) noexcept
        : data_(other.data_),
          size_(other.size_),
          capacity_(other.capacity_),
          front_(other.front_),
          rear_(other.rear_) {

        other.data_ = nullptr;
        other.size_ = 0;
        other.capacity_ = 0;
        other.front_ = 0;
        other.rear_ = 0;
    }

    // 6️⃣ Move assignment
    Queue& operator=(Queue&& other) noexcept {
        if (this != &other) {
            delete[] data_;

            data_ = other.data_;
            size_ = other.size_;
            capacity_ = other.capacity_;
            front_ = other.front_;
            rear_ = other.rear_;

            other.data_ = nullptr;
            other.size_ = 0;
            other.capacity_ = 0;
            other.front_ = 0;
            other.rear_ = 0;
        }
        return *this;
    }

    // 7️⃣ push (enqueue)
    void push(const T& value) {
        if (size_ == capacity_) {
            size_t newCapacity = (capacity_ == 0) ? 1 : capacity_ * 2;
            reallocate(newCapacity);
        }

        data_[rear_] = value;
        rear_ = (rear_ + 1) % capacity_;
        ++size_;
    }

    // 8️⃣ pop (dequeue)
    void pop() {
        assert(size_ > 0 && "Queue underflow");

        front_ = (front_ + 1) % capacity_;
        --size_;
    }

    // 9️⃣ front()
    T& front() {
        assert(size_ > 0 && "Queue is empty");
        return data_[front_];
    }

    const T& front() const {
        assert(size_ > 0 && "Queue is empty");
        return data_[front_];
    }

    // 🔟 size()
    size_t size() const {
        return size_;
    }

    // 1️⃣1️⃣ empty()
    bool empty() const {
        return size_ == 0;
    }
};
```

---

# 🔹 main() – Simulation

```cpp
int main() {

    Queue<int> q;

    // 1️⃣ Enqueue elements
    q.push(10);
    q.push(20);
    q.push(30);

    std::cout << "Front: " << q.front() << "\n";  // 10
    std::cout << "Size: " << q.size() << "\n";

    // 2️⃣ Dequeue
    q.pop();
    std::cout << "Front after pop: " << q.front() << "\n";  // 20

    // 3️⃣ Copy queue
    Queue<int> q2 = q;
    std::cout << "Copied queue front: " << q2.front() << "\n";

    // 4️⃣ Move queue
    Queue<int> q3 = std::move(q2);
    std::cout << "Moved queue front: " << q3.front() << "\n";

    return 0;
}
```

---

# 🔹 Output

```
Front: 10
Size: 3
Front after pop: 20
Copied queue front: 20
Moved queue front: 20
```

---

# 🔹 How It Works Internally

Queue maintains:

```cpp
T* data_;
size_t size_;
size_t capacity_;
size_t front_;
size_t rear_;
```

* `front_` → index of first element
* `rear_` → index where next element will be inserted
* Circular logic using `% capacity_`
* No element shifting required

---

# 🔹 Time Complexity

| Operation | Complexity     |
| --------- | -------------- |
| push      | O(1) amortized |
| pop       | O(1)           |
| front     | O(1)           |
| resize    | O(n)           |

---

# 🔹 Interview-Level Explanation

If interviewer asks:

> Explain your queue implementation.

You say:

> I implemented a FIFO queue using a circular dynamic array to avoid element shifting. The queue maintains front and rear indices and wraps around using modulo arithmetic. When full, it doubles capacity to maintain amortized constant-time insertion. I implemented Rule of Five for proper memory management and move semantics for efficiency.

---

# 🔹 What Production std::queue Does

`std::queue` is actually an adapter:

```cpp
template<class T, class Container = std::deque<T>>
class queue;
```

Default underlying container → `std::deque`

---
