# [Daily Temperatures](#daily-temperatures)

## 🟢 Daily Temperatures

---

### 📌 Problem

Given an array `temperatures`, return an array `answer` such that:

```id="def"
answer[i] = number of days until a warmer temperature
```

If no warmer future day exists:

```id="no"
answer[i] = 0
```

---

### 📌 Example

```id="ex1"
Input:  [73,74,75,71,69,72,76,73]
Output: [1,1,4,2,1,1,0,0]
```

Explanation:

* 73 → next warmer 74 (1 day)
* 75 → next warmer 76 (4 days)
* 76 → none → 0

---

```id="ex2"
Input: [30,40,50,60]
Output: [1,1,1,0]
```

---

# 🔴 1. Brute Force (Check All Future Days)

---

### 💡 Idea

For each day:

* Scan right until warmer day found.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n = temperatures.size();
        vector<int> result(n, 0);

        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < n; j++) {
                if(temperatures[j] > temperatures[i]) {
                    result[i] = j - i;
                    break;
                }
            }
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(n²)

### 🗂 Space Complexity

O(1)

❌ Too slow.

---

# 🟡 2. Better Solution (Monotonic Stack – Forward)

---

### 💡 Key Insight

We need:

```id="need"
Next Greater Element
```

Use **monotonic decreasing stack**.

Stack stores indices of temperatures.

Algorithm:

* Traverse array.
* While current temperature > temperature at stack top:

  * Pop index
  * Compute difference

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n = temperatures.size();
        vector<int> result(n, 0);
        stack<int> st; // store indices

        for(int i = 0; i < n; i++) {
            while(!st.empty() &&
                  temperatures[i] > temperatures[st.top()]) {

                int prevIndex = st.top();
                st.pop();
                result[prevIndex] = i - prevIndex;
            }

            st.push(i);
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(n)

Each element pushed & popped once.

### 🗂 Space Complexity

O(n)

⭐ Standard optimal solution.

---

# 🟢 3. Alternative (Monotonic Stack – Backward)

---

### 💡 Idea

Traverse from right to left.

Maintain stack of indices with strictly greater temperatures.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n = temperatures.size();
        vector<int> result(n, 0);
        stack<int> st;

        for(int i = n - 1; i >= 0; i--) {

            while(!st.empty() &&
                  temperatures[i] >= temperatures[st.top()]) {
                st.pop();
            }

            if(!st.empty())
                result[i] = st.top() - i;

            st.push(i);
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(n)

### 🗂 Space Complexity

O(n)

⭐ Equally optimal.

---

# 🔍 Dry Run

Example:

```id="dry1"
[73,74,75,71,69,72,76,73]
```

Process:

| Day | Temp                 | Stack | Action |
| --- | -------------------- | ----- | ------ |
| 73  | push                 |       |        |
| 74  | pop 73 → result[0]=1 |       |        |
| 75  | pop 74 → result[1]=1 |       |        |
| 71  | push                 |       |        |
| 69  | push                 |       |        |
| 72  | pop 69, 71           |       |        |
| 76  | pop 72, 75           |       |        |
| 73  | push                 |       |        |

Final:

```id="dry2"
[1,1,4,2,1,1,0,0]
```

---

# 🎯 Why Monotonic Stack?

Because we need:

```id="pattern"
Next Greater Element to the right
```

This is a classic pattern.

---

# 🏆 Final Comparison

| Approach        | Time  | Space | Recommended |
| --------------- | ----- | ----- | ----------- |
| Brute Force     | O(n²) | O(1)  | ❌           |
| Monotonic Stack | O(n)  | O(n)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Monotonic stack pattern
* Next greater element concept
* Efficient range queries

Related problems:

* Next Greater Element I & II
* Largest Rectangle in Histogram
* Trapping Rain Water
* Stock Span Problem

---

