# [Implement Max Stack](#Implement-Max-Stack)




## 📌 Problem Definition

Design a stack that supports:

```text
push(x)
pop()
top()
peekMax()     → return maximum element
popMax()      → remove and return maximum element
```

All operations should be as efficient as possible.

---

## 🔥 Key Challenge

Normal stack supports:

```text
push → O(1)
pop  → O(1)
top  → O(1)
```

But:

```text
peekMax()
popMax()
```

If we scan entire stack → O(n) ❌

We need a better approach.

---

## ✅ Approach 1: Two Stacks (Simple Version)

---

### 💡 Idea

Maintain:

```text
mainStack → stores elements
maxStack  → stores maximum so far
```

When pushing:

* Push into mainStack
* If maxStack empty OR val >= maxStack.top()
  → push into maxStack

When popping:

* If popped value == maxStack.top()
  → pop from maxStack too

---

### ⚠️ Limitation

This gives:

```text
peekMax() → O(1)
```

But:

```text
popMax()
```

If max not at top → need to pop elements until max found → O(n)

So this approach works but popMax is O(n).

---

## 💻 C++ Code (Two Stack Approach)

```cpp
#include <iostream>
#include <stack>
using namespace std;

class MaxStack {
private:
    stack<int> mainStack;
    stack<int> maxStack;

public:
    void push(int val) {
        mainStack.push(val);

        if (maxStack.empty() || val >= maxStack.top())
            maxStack.push(val);
    }

    void pop() {
        if (mainStack.empty()) {
            cout << "Underflow\n";
            return;
        }

        if (mainStack.top() == maxStack.top())
            maxStack.pop();

        mainStack.pop();
    }

    int top() {
        return mainStack.empty() ? -1 : mainStack.top();
    }

    int peekMax() {
        return maxStack.empty() ? -1 : maxStack.top();
    }

    // O(n)
    int popMax() {
        if (mainStack.empty()) return -1;

        int maxVal = peekMax();
        stack<int> temp;

        while (mainStack.top() != maxVal) {
            temp.push(mainStack.top());
            mainStack.pop();
        }

        // Remove max
        mainStack.pop();
        maxStack.pop();

        // Push back others
        while (!temp.empty()) {
            push(temp.top());
            temp.pop();
        }

        return maxVal;
    }
};

int main() {

    MaxStack st;

    st.push(5);
    st.push(1);
    st.push(5);

    cout << st.peekMax() << endl; // 5
    cout << st.popMax() << endl;  // 5
    cout << st.top() << endl;     // 1
    cout << st.peekMax() << endl; // 5

    return 0;
}
```

---

## ⏱ Time Complexity (Two Stack)

| Operation | Complexity |
| --------- | ---------- |
| push      | O(1)       |
| pop       | O(1)       |
| top       | O(1)       |
| peekMax   | O(1)       |
| popMax    | O(n)       |

---

## 🔥 Approach 2: Optimized (Doubly Linked List + Ordered Map)

---

### 💡 Idea

Use:

```text
1. Doubly Linked List → maintain stack order
2. map<int, vector<Node*>> → track nodes by value
```

This allows:

* O(1) removal of any node
* O(log n) max lookup

---

### 🔹 Structure

* DLL head = bottom
* DLL tail = top
* Map sorted by key (value)

So:

```text
peekMax() → last key in map
popMax()  → remove last node in map[maxVal]
```

---

### ⏱ Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| push      | O(log n)   |
| pop       | O(log n)   |
| top       | O(1)       |
| peekMax   | O(1)       |
| popMax    | O(log n)   |

This is optimal version.

---

