
# [Implement Queue using Arrays](#Implement-Queue-using-Arrays)

## 📌 Problem Definition

Design a **Queue** data structure using arrays.

Queue follows:

```text
FIFO (First In First Out)
```

Operations to support:

* `enqueue(x)` → Insert element at rear
* `dequeue()` → Remove element from front
* `front()` → Get front element
* `isEmpty()`
* `isFull()`

---

## ⚠️ Important Design Choice

There are **two ways** to implement queue using arrays:

1️⃣ Naive Linear Queue (has wasted space problem)
2️⃣ Circular Queue (Optimal & Expected in interviews)

We’ll implement both.

---

## 🔹 1️⃣ Naive Linear Queue (Basic but Flawed)

---

### 💡 Idea

Maintain:

```text
arr[]
front
rear
capacity
```

Initialization:

```text
front = 0
rear = -1
```

---

### 🚨 Problem

After several dequeues, space at beginning is wasted.

Example:

```text
[ _, _, 30, 40, 50 ]
```

Even if space exists at front, queue appears full.

---

### 💻 C++ Code (Naive Version)

```cpp
#include <iostream>
using namespace std;

class Queue {
private:
    int* arr;
    int frontIndex;
    int rearIndex;
    int capacity;

public:
    Queue(int size) {
        capacity = size;
        arr = new int[capacity];
        frontIndex = 0;
        rearIndex = -1;
    }

    ~Queue() {
        delete[] arr;
    }

    bool isEmpty() {
        return rearIndex < frontIndex;
    }

    bool isFull() {
        return rearIndex == capacity - 1;
    }

    void enqueue(int x) {
        if (isFull()) {
            cout << "Queue Overflow\n";
            return;
        }
        arr[++rearIndex] = x;
    }

    void dequeue() {
        if (isEmpty()) {
            cout << "Queue Underflow\n";
            return;
        }
        frontIndex++;
    }

    int front() {
        if (isEmpty()) return -1;
        return arr[frontIndex];
    }
};

int main() {
    Queue q(5);

    q.enqueue(10);
    q.enqueue(20);
    q.enqueue(30);

    cout << "Front: " << q.front() << endl;

    q.dequeue();
    cout << "Front after dequeue: " << q.front() << endl;

    return 0;
}
```

---

## ⏱ Time Complexity

All operations:

```
O(1)
```

But space is inefficient.

---

## 🔥 2️⃣ Circular Queue (Optimal Approach)

---

## 💡 Core Idea

When `rear` reaches end:

```text
Wrap around using modulo
```

Index movement:

```text
rear = (rear + 1) % capacity
front = (front + 1) % capacity
```

---

## 🔹 Circular Queue Conditions

Empty:

```text
size == 0
```

Full:

```text
size == capacity
```

---

## 💻 C++ Code (Circular Queue – Interview Ready)

```cpp
#include <iostream>
using namespace std;

class CircularQueue {
private:
    int* arr;
    int frontIndex;
    int rearIndex;
    int size;
    int capacity;

public:
    CircularQueue(int cap) {
        capacity = cap;
        arr = new int[capacity];
        frontIndex = 0;
        rearIndex = -1;
        size = 0;
    }

    ~CircularQueue() {
        delete[] arr;
    }

    bool isEmpty() {
        return size == 0;
    }

    bool isFull() {
        return size == capacity;
    }

    void enqueue(int x) {
        if (isFull()) {
            cout << "Queue Overflow\n";
            return;
        }

        rearIndex = (rearIndex + 1) % capacity;
        arr[rearIndex] = x;
        size++;
    }

    void dequeue() {
        if (isEmpty()) {
            cout << "Queue Underflow\n";
            return;
        }

        frontIndex = (frontIndex + 1) % capacity;
        size--;
    }

    int front() {
        if (isEmpty()) return -1;
        return arr[frontIndex];
    }

    int getSize() {
        return size;
    }
};

int main() {

    CircularQueue q(5);

    q.enqueue(10);
    q.enqueue(20);
    q.enqueue(30);

    cout << "Front: " << q.front() << endl;

    q.dequeue();
    cout << "Front after dequeue: " << q.front() << endl;

    q.enqueue(40);
    q.enqueue(50);
    q.enqueue(60);

    cout << "Queue size: " << q.getSize() << endl;

    return 0;
}
```

---

## 🔥 Why Circular Queue Is Better

Without circular behavior:

```text
Space gets wasted.
```

With circular logic:

```text
Space is reused efficiently.
```

---

## ⏱ Time Complexity

All operations:

```
O(1)
```

---

## 🧠 Space Complexity

```
O(n)
```

---

## 🔥 Interview Discussion Points

You should mention:

* Why modulo is used
* Why maintain `size`
* Difference between:

  * `(rear + 1) % capacity == front` approach
  * Using size variable
* Why naive approach wastes space
* How STL queue works internally (deque-based)

---

## 🔥 STL Equivalent

```cpp
#include <queue>

queue<int> q;
q.push(10);
q.pop();
```

---

## 🎯 Final Comparison

| Implementation | Time | Space | Interview Rating |
| -------------- | ---- | ----- | ---------------- |
| Linear Queue   | O(1) | O(n)  | ❌ Not ideal      |
| Circular Queue | O(1) | O(n)  | ⭐ Expected       |

---
