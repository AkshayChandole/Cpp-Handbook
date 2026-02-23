# [Implement queue using Linkedlist](#Implement-queue-using-Linkedlist)


## 📌 Problem Definition

Implement a **Queue (FIFO)** using a **Singly Linked List**.

Queue supports:

```text
enqueue(x)
dequeue()
front()
isEmpty()
size()
```

Queue follows:

```text
FIFO → First In First Out
```

---

## 🔥 Why Use Linked List?

| Array Queue          | Linked List Queue      |
| -------------------- | ---------------------- |
| Fixed capacity       | Dynamic size           |
| Needs circular logic | Naturally dynamic      |
| Possible overflow    | Only limited by memory |

Linked list queue avoids resizing & modulo logic.

---

## 🧠 Core Idea

We maintain **two pointers**:

```text
front → first element
rear  → last element
```

Example after enqueue 10, 20, 30:

```text
Front                    Rear
  ↓                        ↓
10 → 20 → 30 → NULL
```

---

## 🔹 Structure Design

```text
Node:
    data
    next
```

Queue:

* Node* front
* Node* rear
* int size

---

## ✅ C++ Implementation (Memory Safe)

---

```cpp
#include <iostream>
using namespace std;

// Node definition
struct Node {
    int data;
    Node* next;

    Node(int val) {
        data = val;
        next = nullptr;
    }
};

class Queue {
private:
    Node* frontPtr;
    Node* rearPtr;
    int count;

public:
    Queue() {
        frontPtr = nullptr;
        rearPtr = nullptr;
        count = 0;
    }

    ~Queue() {
        while (!isEmpty())
            dequeue();
    }

    void enqueue(int x) {
        Node* newNode = new Node(x);

        if (isEmpty()) {
            frontPtr = rearPtr = newNode;
        } else {
            rearPtr->next = newNode;
            rearPtr = newNode;
        }

        count++;
    }

    void dequeue() {
        if (isEmpty()) {
            cout << "Queue Underflow\n";
            return;
        }

        Node* temp = frontPtr;
        frontPtr = frontPtr->next;

        if (frontPtr == nullptr)
            rearPtr = nullptr;

        delete temp;
        count--;
    }

    int front() {
        if (isEmpty())
            return -1;

        return frontPtr->data;
    }

    bool isEmpty() {
        return frontPtr == nullptr;
    }

    int size() {
        return count;
    }
};

int main() {

    Queue q;

    q.enqueue(10);
    q.enqueue(20);
    q.enqueue(30);

    cout << "Front: " << q.front() << endl;

    q.dequeue();
    cout << "Front after dequeue: " << q.front() << endl;

    cout << "Size: " << q.size() << endl;

    return 0;
}
```

---

## 🔥 Operation Breakdown

---

### 🔹 enqueue(x)

Steps:

```text
1. Create new node
2. If queue empty → front = rear = newNode
3. Else → rear->next = newNode
4. Move rear to newNode
```

Time:

```
O(1)
```

---

### 🔹 dequeue()

Steps:

```text
1. Store front in temp
2. Move front to front->next
3. If front becomes NULL → rear = NULL
4. Delete temp
```

Time:

```
O(1)
```

---

### 🔹 front()

```
Return frontPtr->data
```

Time:

```
O(1)
```

---

## ⏱ Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| enqueue   | O(1)       |
| dequeue   | O(1)       |
| front     | O(1)       |
| isEmpty   | O(1)       |

---

## 🧠 Space Complexity

```
O(n)
```

Each node stores:

* data
* next pointer

---

## 🔥 Important Edge Case

When last element is dequeued:

```cpp
if (frontPtr == nullptr)
    rearPtr = nullptr;
```

If not handled → rear becomes dangling pointer.

---

## 🔥 Why Maintain Both front and rear?

If only front is maintained:

* Enqueue becomes O(n) (need traversal)

Using rear ensures:

```
O(1) enqueue
```

---

## 🔥 Comparison with Stack Using Linked List

| Stack              | Queue               |
| ------------------ | ------------------- |
| Insert at head     | Insert at tail      |
| Delete from head   | Delete from head    |
| One pointer enough | Two pointers needed |

---

## 🎯 Final Summary

Queue using linked list:

```
front → first node
rear  → last node

enqueue → insert at rear
dequeue → remove from front
All operations → O(1)
```

---
