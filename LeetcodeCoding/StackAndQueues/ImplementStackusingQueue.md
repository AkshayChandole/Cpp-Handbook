# [Implement Stack using Queue](#Implement-Stack-using-Queue)

## 📌 Problem Definition

Implement a **Stack (LIFO)** using **Queue (FIFO)** operations.

Stack supports:

```text
push(x)
pop()
top()
empty()
```

Queue supports only:

```text
push()   // enqueue
pop()    // dequeue
front()
size()
```

---

## 🔥 Core Idea

Since:

```text
Stack → LIFO
Queue → FIFO
```

We must simulate LIFO behavior using FIFO structure.

---

## ✅ Approach 1: Using TWO Queues (Simple Logic)

---

### 💡 Idea

For `push(x)`:

1. Push element into `q2`
2. Move all elements from `q1` to `q2`
3. Swap `q1` and `q2`

This ensures:

```text
Newest element always at front of q1
```

So pop becomes easy.

---

### 💻 C++ Code (Two Queues)

```cpp
#include <iostream>
#include <queue>
using namespace std;

class Stack {
private:
    queue<int> q1, q2;

public:
    void push(int x) {
        // Step 1: Push to q2
        q2.push(x);

        // Step 2: Move all from q1 to q2
        while (!q1.empty()) {
            q2.push(q1.front());
            q1.pop();
        }

        // Step 3: Swap q1 and q2
        swap(q1, q2);
    }

    void pop() {
        if (q1.empty()) {
            cout << "Stack Underflow\n";
            return;
        }
        q1.pop();
    }

    int top() {
        if (q1.empty()) return -1;
        return q1.front();
    }

    bool empty() {
        return q1.empty();
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

    return 0;
}
```

---

## ⏱ Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| push      | O(n)       |
| pop       | O(1)       |
| top       | O(1)       |

---

## 🔥 Approach 2: Using ONE Queue (Optimized)

---

### 💡 Core Trick

After pushing new element:

Rotate the queue so that new element moves to front.

Steps:

```text
1. Push x
2. Rotate previous elements (size - 1 times)
```

---

### 💻 C++ Code (Single Queue – Interview Favorite)

```cpp
#include <iostream>
#include <queue>
using namespace std;

class Stack {
private:
    queue<int> q;

public:
    void push(int x) {
        q.push(x);

        int size = q.size();

        // Rotate previous elements
        for (int i = 0; i < size - 1; i++) {
            q.push(q.front());
            q.pop();
        }
    }

    void pop() {
        if (q.empty()) {
            cout << "Stack Underflow\n";
            return;
        }
        q.pop();
    }

    int top() {
        if (q.empty()) return -1;
        return q.front();
    }

    bool empty() {
        return q.empty();
    }
};

int main() {
    Stack st;

    st.push(1);
    st.push(2);
    st.push(3);

    cout << "Top: " << st.top() << endl;

    st.pop();
    cout << "Top after pop: " << st.top() << endl;

    return 0;
}
```

---

## 🔥 Why Rotation Works

Example:

Initial queue after pushing 1,2:

```text
[1,2]
```

Now push 3:

```text
[1,2,3]
```

Rotate twice:

```text
Step 1 → [2,3,1]
Step 2 → [3,1,2]
```

Now top element (3) is at front → behaves like stack.

---

## ⏱ Time Complexity (Single Queue)

| Operation | Complexity |
| --------- | ---------- |
| push      | O(n)       |
| pop       | O(1)       |
| top       | O(1)       |

---

## 🔥 Alternative Strategy (Make pop costly instead)

We can also:

* Keep push O(1)
* Make pop O(n)

But interviewers prefer:

```text
Costly push, cheap pop
```

---

## 🧠 Space Complexity

```text
O(n)
```

---

## 🔥 Interview Discussion Points

You should mention:

* Two approaches
* Trade-off between push and pop
* Why rotation simulates LIFO
* STL queue operations used
* Time complexity difference

---

## 🎯 Final Comparison

| Approach   | Push | Pop  | Space | Preference |
| ---------- | ---- | ---- | ----- | ---------- |
| Two Queues | O(n) | O(1) | O(n)  | Good       |
| One Queue  | O(n) | O(1) | O(n)  | ⭐ Best     |

---
