# [Basic Calculator II](#basic-calculator-ii)

## 🟢 Basic Calculator II

---

### 📌 Problem

Given a string `s` representing a valid expression, evaluate it and return the result.

The expression contains:

* Non-negative integers
* `+`, `-`, `*`, `/`
* Spaces

Rules:

* Division truncates toward zero.
* No parentheses.
* Evaluate with correct operator precedence.

---

### 📌 Example

```id="ex1"
Input: s = "3+2*2"
Output: 7
```

```id="ex2"
Input: s = " 3/2 "
Output: 1
```

```id="ex3"
Input: s = " 3+5 / 2 "
Output: 5
```

---

# 🔴 1. Brute Force (Two Pass Evaluation)

---

### 💡 Idea

1. First pass → handle `*` and `/`
2. Second pass → handle `+` and `-`

---

### ❌ Drawback

* Complex parsing
* Multiple passes
* Extra space

---

### ⏱ Time Complexity

O(n)

But implementation messy.

---

# 🟡 2. Better Solution (Stack Based)

---

### 💡 Key Insight

Use stack to handle precedence.

Algorithm:

1. Traverse string.
2. Build current number.
3. When operator encountered:

   * If previous operator was:

     * `+` → push number
     * `-` → push -number
     * `*` → pop, multiply, push result
     * `/` → pop, divide, push result
4. At end → sum stack.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    int calculate(string s) {
        stack<int> st;
        int num = 0;
        char op = '+';

        for(int i = 0; i < s.size(); i++) {
            if(isdigit(s[i])) {
                num = num * 10 + (s[i] - '0');
            }

            if((!isdigit(s[i]) && s[i] != ' ') || i == s.size() - 1) {

                if(op == '+')
                    st.push(num);
                else if(op == '-')
                    st.push(-num);
                else if(op == '*') {
                    int top = st.top(); st.pop();
                    st.push(top * num);
                }
                else if(op == '/') {
                    int top = st.top(); st.pop();
                    st.push(top / num);
                }

                op = s[i];
                num = 0;
            }
        }

        int result = 0;
        while(!st.empty()) {
            result += st.top();
            st.pop();
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

✅ Standard solution.

---

# 🟢 3. Optimum Solution (Without Stack – Constant Space)

---

### 💡 Observation

Instead of stack, maintain:

* `currentNumber`
* `lastNumber`
* `result`

Idea:

* Add previous number to result when encountering `+` or `-`
* For `*` and `/`, modify `lastNumber`

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    int calculate(string s) {
        long num = 0;
        long lastNumber = 0;
        long result = 0;
        char op = '+';

        for(int i = 0; i < s.size(); i++) {

            if(isdigit(s[i])) {
                num = num * 10 + (s[i] - '0');
            }

            if((!isdigit(s[i]) && s[i] != ' ') || i == s.size() - 1) {

                if(op == '+') {
                    result += lastNumber;
                    lastNumber = num;
                }
                else if(op == '-') {
                    result += lastNumber;
                    lastNumber = -num;
                }
                else if(op == '*') {
                    lastNumber = lastNumber * num;
                }
                else if(op == '/') {
                    lastNumber = lastNumber / num;
                }

                op = s[i];
                num = 0;
            }
        }

        result += lastNumber;
        return result;
    }
};
```

---

### ⏱ Time Complexity

O(n)

### 🗂 Space Complexity

O(1)

⭐ Most optimal solution.

---

# 🔍 Why This Works

We delay adding to result until:

* We are sure no higher-precedence operator affects it.

`lastNumber` holds pending value.

---

# 🔎 Dry Run

Example:

```id="dry1"
"3+2*2"
```

Step-by-step:

| num | op | lastNumber | result |
| --- | -- | ---------- | ------ |
| 3   | +  | 3          | 0      |
| 2   | +  | 2          | 3      |
| 2   | *  | 4          | 3      |

Final:

```id="dry2"
result = 3 + 4 = 7
```

---

# 🎯 Important Edge Cases

* Multiple digits
* Leading/trailing spaces
* Division truncation
* Last number handling

---

# 🏆 Final Comparison

| Approach | Time | Space | Recommended |
| -------- | ---- | ----- | ----------- |
| Two Pass | O(n) | O(n)  | ❌           |
| Stack    | O(n) | O(n)  | ✅           |
| No Stack | O(n) | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Expression parsing
* Operator precedence
* Stack simulation
* String processing

Related problems:

* Basic Calculator I (with parentheses)
* Basic Calculator III
* Evaluate Reverse Polish Notation
* Decode String

---

