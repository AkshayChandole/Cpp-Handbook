
# [Implement Stack using Arrays](#implement-stack-using-arrays)


## 📌 Problem Definition

Design a **Stack** data structure using arrays.

Stack follows:

```text
LIFO (Last In First Out)
```

Operations to support:

* `push(x)` → Insert element
* `pop()` → Remove top element
* `top()` → Return top element
* `isEmpty()` → Check if empty
* `isFull()` → Check if full (for fixed size)

---

## 🔹 Stack Structure (Using Array)

We maintain:

```text
arr[] → storage
top   → index of top element
capacity → maximum size
```

Initialization:

```text
top = -1
```

---

## ✅ 1️⃣ Basic Fixed Size Stack (Using Static Array)

---

### 💻 C++ Code

```cpp
#include <iostream>
using namespace std;

class Stack {
private:
    int* arr;
    int topIndex;
    int capacity;

public:
    Stack(int size) {
        capacity = size;
        arr = new int[capacity];
        topIndex = -1;
    }

    ~Stack() {
        delete[] arr;
    }

    bool isEmpty() {
        return topIndex == -1;
    }

    bool isFull() {
        return topIndex == capacity - 1;
    }

    void push(int x) {
        if (isFull()) {
            cout << "Stack Overflow\n";
            return;
        }
        arr[++topIndex] = x;
    }

    void pop() {
        if (isEmpty()) {
            cout << "Stack Underflow\n";
            return;
        }
        topIndex--;
    }

    int top() {
        if (isEmpty()) {
            cout << "Stack is empty\n";
            return -1;
        }
        return arr[topIndex];
    }

    int size() {
        return topIndex + 1;
    }
};

int main() {

    Stack st(5);

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

## 🔥 Operations Explanation

---

### 🔹 push(x)

```text
1. Check overflow
2. Increment top
3. Insert element
```

```cpp
arr[++topIndex] = x;
```

---

### 🔹 pop()

```text
1. Check underflow
2. Decrement top
```

---

### 🔹 top()

```text
Return arr[topIndex]
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

---

## ⚠️ Problem with Fixed Array

If capacity is full → cannot push more elements.

---

## ✅ 2️⃣ Dynamic Stack (Auto-Resizing Like Vector)

---

### 💡 Idea

If stack becomes full:

```text
Double the capacity
```

---

### 💻 C++ Code (Dynamic Resizing)

```cpp
#include <iostream>
using namespace std;

class Stack {
private:
    int* arr;
    int topIndex;
    int capacity;

    void resize() {
        int newCapacity = capacity * 2;
        int* newArr = new int[newCapacity];

        for (int i = 0; i <= topIndex; i++)
            newArr[i] = arr[i];

        delete[] arr;
        arr = newArr;
        capacity = newCapacity;
    }

public:
    Stack(int size = 4) {
        capacity = size;
        arr = new int[capacity];
        topIndex = -1;
    }

    ~Stack() {
        delete[] arr;
    }

    void push(int x) {
        if (topIndex == capacity - 1)
            resize();

        arr[++topIndex] = x;
    }

    void pop() {
        if (topIndex == -1) {
            cout << "Stack Underflow\n";
            return;
        }
        topIndex--;
    }

    int top() {
        if (topIndex == -1)
            return -1;

        return arr[topIndex];
    }

    bool isEmpty() {
        return topIndex == -1;
    }
};

int main() {

    Stack st;

    for (int i = 1; i <= 10; i++)
        st.push(i);

    while (!st.isEmpty()) {
        cout << st.top() << " ";
        st.pop();
    }

    return 0;
}
```

---

## 🔥 Amortized Analysis

Even though resize is O(n):

Overall amortized push cost:

```text
O(1)
```

---

## 🔥 Interview-Level Discussion Points

You should mention:

* Why top starts at -1
* Overflow and Underflow
* Why resizing doubles capacity
* Amortized O(1) analysis
* Memory management (Rule of 3/5)

---

## 🔥 Edge Cases

* Pop from empty stack
* Push into full stack
* Large number of pushes
* Memory leaks (handle destructor)

---

## 🔥 STL Equivalent

C++ already provides:

```cpp
#include <stack>

stack<int> st;
```

But interview expects manual implementation.

---

## 🎯 Final Comparison

| Type       | Time           | Space | Notes            |
| ---------- | -------------- | ----- | ---------------- |
| Fixed Size | O(1)           | O(n)  | Simple           |
| Dynamic    | Amortized O(1) | O(n)  | Production ready |

---

