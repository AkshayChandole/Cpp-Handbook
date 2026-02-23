# [Implement stack using Linkedlist](#Implement-stack-using-Linkedlist)


## 📌 Problem Definition

Implement a **Stack (LIFO)** using a **Singly Linked List**.

Stack supports:

```text
push(x)
pop()
top()
isEmpty()
size()
```

---

## 🔥 Why Use Linked List Instead of Array?

| Array Stack                 | Linked List Stack     |
| --------------------------- | --------------------- |
| Fixed size (unless resized) | Dynamic size          |
| Possible overflow if full   | Grows dynamically     |
| Contiguous memory           | Non-contiguous memory |

Linked list avoids capacity limit.

---

## 🧠 Core Idea

We maintain:

```text
head → top of stack
```

Why?

Because:

* Insert at head → O(1)
* Delete at head → O(1)

Perfect for stack.

---

## 🔹 Stack Structure

```text
Top → Head of Linked List
```

Example after pushing 10, 20, 30:

```text
Top
 ↓
30 → 20 → 10 → NULL
```

---

## ✅ C++ Implementation

---

### 💻 Full Code (Memory Safe)

```cpp
#include <iostream>
using namespace std;

// Node structure
struct Node {
    int data;
    Node* next;

    Node(int val) {
        data = val;
        next = nullptr;
    }
};

class Stack {
private:
    Node* head;   // top of stack
    int count;

public:
    Stack() {
        head = nullptr;
        count = 0;
    }

    ~Stack() {
        while (!isEmpty())
            pop();
    }

    void push(int x) {
        Node* newNode = new Node(x);

        newNode->next = head;
        head = newNode;

        count++;
    }

    void pop() {
        if (isEmpty()) {
            cout << "Stack Underflow\n";
            return;
        }

        Node* temp = head;
        head = head->next;
        delete temp;

        count--;
    }

    int top() {
        if (isEmpty())
            return -1;

        return head->data;
    }

    bool isEmpty() {
        return head == nullptr;
    }

    int size() {
        return count;
    }
};

int main() {

    Stack st;

    st.push(10);
    st.push(20);
    st.push(30);

    cout << "Top: " << st.top() << endl;

    st.pop();
    cout << "Top after pop: " << st.top() << endl;

    cout << "Size: " << st.size() << endl;

    return 0;
}
```

---

## 🔥 Operation Breakdown

---

### 🔹 push(x)

Steps:

```text
1. Create new node
2. Point newNode->next to head
3. Move head to newNode
```

Time:

```text
O(1)
```

---

### 🔹 pop()

Steps:

```text
1. Store head in temp
2. Move head to head->next
3. Delete temp
```

Time:

```text
O(1)
```

---

### 🔹 top()

```text
Return head->data
```

Time:

```text
O(1)
```

---

## ⏱ Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| push      | O(1)       |
| pop       | O(1)       |
| top       | O(1)       |
| isEmpty   | O(1)       |

---

## 🧠 Space Complexity

```text
O(n)
```

Each node stores:

* data
* next pointer

---

## 🔥 Why Insert/Delete at Head?

If we insert at tail:

* We need traversal
* Time becomes O(n)

Head gives constant time.

---


## 🔥 Comparison With Array Stack

| Feature         | Array  | Linked List            |
| --------------- | ------ | ---------------------- |
| Dynamic Growth  | ❌      | ✅                      |
| Cache Friendly  | ✅      | ❌                      |
| Memory Overhead | Low    | Higher (extra pointer) |
| Implementation  | Easier | Slightly complex       |

---

## 🎯 Final Summary

Stack using linked list:

```text
Top → Head
Push → Insert at head
Pop → Remove from head
All operations O(1)
```

---
