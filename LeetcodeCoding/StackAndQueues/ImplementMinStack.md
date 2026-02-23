# [Implement Min Stack](#Implement-Min-Stack)


## 📌 Problem Definition

Design a stack that supports:

```text
push(x)
pop()
top()
getMin()
```

All operations must run in:

```text
O(1) time
```

---

## 🔥 Key Challenge

Normal stack can give:

```text
push → O(1)
pop  → O(1)
top  → O(1)
```

But:

```text
getMin()
```

If we scan stack → O(n) ❌ Not allowed.

So we must store minimum efficiently.

---

## ✅ Approach 1: Two Stacks (Most Common & Clean)

---

### 💡 Idea

Maintain:

```text
mainStack → stores all elements
minStack  → stores minimums
```

Rule:

* When pushing:

  * Push into mainStack.
  * If minStack empty OR x <= minStack.top()
    → Push x into minStack.
* When popping:

  * If popped element == minStack.top()
    → pop from minStack too.

---

### 🔥 Why This Works

minStack always keeps:

```text
Current minimum at top
```

---

### 💻 C++ Code (Two Stack Solution)

```cpp
#include <iostream>
#include <stack>
using namespace std;

class MinStack {
private:
    stack<int> mainStack;
    stack<int> minStack;

public:
    void push(int val) {
        mainStack.push(val);

        if (minStack.empty() || val <= minStack.top()) {
            minStack.push(val);
        }
    }

    void pop() {
        if (mainStack.empty()) {
            cout << "Stack Underflow\n";
            return;
        }

        if (mainStack.top() == minStack.top()) {
            minStack.pop();
        }

        mainStack.pop();
    }

    int top() {
        if (mainStack.empty())
            return -1;

        return mainStack.top();
    }

    int getMin() {
        if (minStack.empty())
            return -1;

        return minStack.top();
    }

    bool empty() {
        return mainStack.empty();
    }
};

int main() {

    MinStack st;

    st.push(3);
    st.push(5);
    cout << "Min: " << st.getMin() << endl;  // 3

    st.push(2);
    st.push(1);
    cout << "Min: " << st.getMin() << endl;  // 1

    st.pop();
    cout << "Min after pop: " << st.getMin() << endl;  // 2

    return 0;
}
```

---

## ⏱ Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| push      | O(1)       |
| pop       | O(1)       |
| top       | O(1)       |
| getMin    | O(1)       |

---

## 🧠 Space Complexity

Worst case:

```text
O(n)
```

Because minStack may store all elements (if strictly decreasing).

---

## 🔥 Approach 2: Optimized Single Stack (Space Efficient)

---

### 💡 Trick

Instead of using two stacks, store **encoded values** when pushing smaller element.

Maintain:

```text
minValue → current minimum
```

When pushing:

If `val >= minValue`:

* Push normally

If `val < minValue`:

* Push encoded value:

```text
2*val - minValue
```

Update:

```text
minValue = val
```

When popping:

If popped value < minValue:

* It means encoded value
* Recover previous min:

```text
previousMin = 2*minValue - encodedValue
```

---

### 🔥 Why This Works

We cleverly store information inside stack itself without extra stack.

---

### 💻 C++ Code (Single Stack Optimized)

```cpp
#include <iostream>
#include <stack>
using namespace std;

class MinStack {
private:
    stack<long long> st;
    long long minVal;

public:
    void push(int val) {
        if (st.empty()) {
            st.push(val);
            minVal = val;
        }
        else if (val >= minVal) {
            st.push(val);
        }
        else {
            // Encode
            st.push(2LL * val - minVal);
            minVal = val;
        }
    }

    void pop() {
        if (st.empty()) {
            cout << "Underflow\n";
            return;
        }

        long long topVal = st.top();
        st.pop();

        if (topVal < minVal) {
            minVal = 2 * minVal - topVal;
        }
    }

    int top() {
        long long topVal = st.top();

        if (topVal < minVal)
            return minVal;

        return topVal;
    }

    int getMin() {
        return minVal;
    }
};

int main() {

    MinStack st;

    st.push(5);
    st.push(3);
    st.push(7);

    cout << "Min: " << st.getMin() << endl; // 3

    st.pop();
    cout << "Min after pop: " << st.getMin() << endl; // 3

    st.pop();
    cout << "Min after pop: " << st.getMin() << endl; // 5

    return 0;
}
```

---

## ⏱ Time Complexity

All operations:

```text
O(1)
```

---

## 🧠 Space Complexity

```text
O(n)
```

But uses only one stack.

---

## 🔥 Interview Comparison

| Approach      | Extra Stack | Space | Complexity | Preference       |
| ------------- | ----------- | ----- | ---------- | ---------------- |
| Two Stacks    | Yes         | O(n)  | Easy       | ⭐ Most common    |
| Encoded Trick | No          | O(n)  | Advanced   | ⭐ Advanced level |

---

