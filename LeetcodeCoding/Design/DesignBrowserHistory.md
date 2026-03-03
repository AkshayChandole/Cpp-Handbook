# [Design Browser History](#design-browser-history)

## 🟢 Design Browser History

---

### 📌 Problem

Design a browser history system that supports:

* `BrowserHistory(string homepage)`
* `visit(string url)`
* `back(int steps)`
* `forward(int steps)`

Behavior:

* Visiting a new page clears forward history.
* `back()` moves backward up to `steps`.
* `forward()` moves forward up to `steps`.

---

### 📌 Example

```id="ex1"
Input:
["BrowserHistory","visit","visit","visit","back","back","forward","visit","forward","back","back"]
[["leetcode.com"],["google.com"],["facebook.com"],["youtube.com"],[1],[1],[1],["linkedin.com"],[2],[2],[7]]

Output:
[null,null,null,null,"facebook.com","google.com",
"facebook.com",null,"linkedin.com","google.com","leetcode.com"]
```

---

# 🔴 1. Brute Force (Vector + Erase Forward History)

### 💡 Idea

* Store history in vector.
* Maintain `currentIndex`.
* On visit:

  * Remove all elements after currentIndex.
  * Push new url.

---

### 💻 Code (C++)

```cpp id="bf1"
class BrowserHistory {
public:
    vector<string> history;
    int current;

    BrowserHistory(string homepage) {
        history.push_back(homepage);
        current = 0;
    }

    void visit(string url) {
        history.erase(history.begin() + current + 1, history.end());
        history.push_back(url);
        current++;
    }

    string back(int steps) {
        current = max(0, current - steps);
        return history[current];
    }

    string forward(int steps) {
        current = min((int)history.size() - 1, current + steps);
        return history[current];
    }
};
```

---

### ⏱ Time Complexity

* `visit()` → O(N) (due to erase)
* `back()` / `forward()` → O(1)

### 🗂 Space Complexity

O(N)

❌ Erase operation is costly.

---

# 🟡 2. Better Solution (Two Stacks)

### 💡 Idea

Maintain:

* `backStack`
* `forwardStack`

---

### 🔹 Rules

* On visit:

  * Push current to backStack.
  * Clear forwardStack.
* On back:

  * Move current to forwardStack.
  * Pop from backStack.
* On forward:

  * Move current to backStack.
  * Pop from forwardStack.

---

### 💻 Code (C++)

```cpp id="better1"
class BrowserHistory {
public:
    stack<string> backStack;
    stack<string> forwardStack;
    string current;

    BrowserHistory(string homepage) {
        current = homepage;
    }

    void visit(string url) {
        backStack.push(current);
        current = url;

        while(!forwardStack.empty())
            forwardStack.pop();
    }

    string back(int steps) {
        while(steps-- && !backStack.empty()) {
            forwardStack.push(current);
            current = backStack.top();
            backStack.pop();
        }
        return current;
    }

    string forward(int steps) {
        while(steps-- && !forwardStack.empty()) {
            backStack.push(current);
            current = forwardStack.top();
            forwardStack.pop();
        }
        return current;
    }
};
```

---

### ⏱ Time Complexity

Each operation → O(steps)

Worst case → O(N)

### 🗂 Space Complexity

O(N)

✅ Conceptually clean.

---

# 🟢 3. Optimum Solution (Vector + Size Pointer Trick)

### 💡 Key Insight

Instead of erase:

Maintain:

```id="ptr"
vector<string> history
int current
int last
```

Where:

* `current` → current page
* `last` → last valid index (end of forward history)

On visit:

* Increment current
* Set history[current] = new url
* Update last = current

No erase needed.

---

### 💻 Code (C++)

```cpp id="opt1"
class BrowserHistory {
public:
    vector<string> history;
    int current;
    int last;

    BrowserHistory(string homepage) {
        history.push_back(homepage);
        current = 0;
        last = 0;
    }

    void visit(string url) {
        current++;

        if(current < history.size())
            history[current] = url;
        else
            history.push_back(url);

        last = current;
    }

    string back(int steps) {
        current = max(0, current - steps);
        return history[current];
    }

    string forward(int steps) {
        current = min(last, current + steps);
        return history[current];
    }
};
```

---

### ⏱ Time Complexity

All operations → O(1)

### 🗂 Space Complexity

O(N)

⭐ Most optimal solution.

---

# 🔍 Why This Works

Instead of deleting forward history:

We just move `last` pointer.

Forward pages still exist in memory but are ignored.

---

# 🏆 Final Comparison

| Approach      | Visit | Back     | Forward  | Recommended |
| ------------- | ----- | -------- | -------- | ----------- |
| Erase Vector  | O(N)  | O(1)     | O(1)     | ❌           |
| Two Stacks    | O(1)  | O(steps) | O(steps) | ✅           |
| Pointer Trick | O(1)  | O(1)     | O(1)     | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Stack simulation
* State management
* Index pointer optimization

Related problems:

* Design Twitter
* LRU Cache
* Min Stack
* Implement Queue using Stacks

---

