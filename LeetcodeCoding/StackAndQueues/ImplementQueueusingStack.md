# [Implement Queue using Stack](#Implement-Queue-using-Stack)



## 📌 Problem Definition

Implement a **Queue (FIFO)** using **Stack (LIFO)** operations.

Queue supports:

```text
enqueue(x)
dequeue()
front()
empty()
```

Stack supports only:

```text
push()
pop()
top()
empty()
```

---

## 🔥 Core Idea

Since:

```
Queue → FIFO
Stack → LIFO
```

We must reverse order twice to simulate FIFO behavior.

---

## ✅ Approach 1: Using TWO Stacks (Optimal & Expected)

---

### 💡 Main Idea

Use two stacks:

```
inStack  → for enqueue
outStack → for dequeue
```

#### enqueue(x)

Push into `inStack`.

#### dequeue()

If `outStack` is empty:

* Move all elements from `inStack` → `outStack`
* This reverses order.

Then pop from `outStack`.

---

### 🔥 Why This Works

Example:

Enqueue: 1,2,3

```
inStack: [1,2,3] (top=3)
```

Transfer to outStack:

```
outStack: [3,2,1] (top=1)
```

Now:

```
pop() → 1  ✅ FIFO
```

---

## 💻 C++ Code (Two Stacks – Amortized O(1))

```cpp
#include <iostream>
#include <stack>
using namespace std;

class Queue {
private:
    stack<int> inStack;
    stack<int> outStack;

    void transfer() {
        while (!inStack.empty()) {
            outStack.push(inStack.top());
            inStack.pop();
        }
    }

public:
    void enqueue(int x) {
        inStack.push(x);
    }

    void dequeue() {
        if (empty()) {
            cout << "Queue Underflow\n";
            return;
        }

        if (outStack.empty())
            transfer();

        outStack.pop();
    }

    int front() {
        if (empty())
            return -1;

        if (outStack.empty())
            transfer();

        return outStack.top();
    }

    bool empty() {
        return inStack.empty() && outStack.empty();
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

    return 0;
}
```

---

## ⏱ Time Complexity

| Operation | Complexity     |
| --------- | -------------- |
| enqueue   | O(1)           |
| dequeue   | Amortized O(1) |
| front     | Amortized O(1) |

---

### 🔥 Why Amortized O(1)?

Each element:

* Moves from `inStack` → `outStack` once
* Popped once

So total operations per element = constant.

Overall:

```
O(n) work for n elements
→ Amortized O(1)
```

---

## 🧠 Space Complexity

```
O(n)
```

---

## 🔥 Approach 2: Make enqueue costly (Less Preferred)

Alternative:

* For enqueue:

  * Move all elements to temp stack
  * Push new element
  * Move everything back

Then dequeue becomes O(1).

But this makes enqueue O(n).

Not preferred in interviews.

---

## 🎯 Final Comparison

| Approach                   | Enqueue | Dequeue        | Preference |
| -------------------------- | ------- | -------------- | ---------- |
| Two Stacks (Lazy transfer) | O(1)    | Amortized O(1) | ⭐ Best     |
| Costly Enqueue             | O(n)    | O(1)           | Less ideal |

---
