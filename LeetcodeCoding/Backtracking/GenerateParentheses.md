# [Generate Parentheses](#generate-parentheses)


## 🧩 Problem: Generate Parentheses

Given `n` pairs of parentheses, write a function to generate **all combinations of well-formed parentheses**.

---

## 🔎 Examples

**Example 1:**

```id="ex1"
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

**Example 2:**

```id="ex2"
Input: n = 1
Output: ["()"]
```

---

# 🐢 Brute Force Solution (Generate All & Validate)

### 💡 Idea

1. Generate all possible strings of length `2n` using `'('` and `')'`.
2. Check each string if it is valid.
3. Collect valid ones.

---

### 🔹 Algorithm

1. Use recursion to generate all combinations of length `2n`.
2. For each generated string:

   * Check if it is valid using balance counter.
3. Store valid results.

---

### ⏱ Time Complexity

* Total combinations = `2^(2n)`
* Validation takes `O(n)`
* Total ≈ **O(n * 2^(2n))**

### 📦 Space Complexity

* **O(n)** recursion stack

---

### 💻 C++ Code

```cpp id="bf1">
class Solution {
public:
    bool isValid(string s) {
        int balance = 0;
        for(char c : s) {
            if(c == '(') balance++;
            else balance--;
            if(balance < 0) return false;
        }
        return balance == 0;
    }

    void generate(string curr, int n, vector<string>& result) {
        if(curr.length() == 2*n) {
            if(isValid(curr))
                result.push_back(curr);
            return;
        }

        generate(curr + "(", n, result);
        generate(curr + ")", n, result);
    }

    vector<string> generateParenthesis(int n) {
        vector<string> result;
        generate("", n, result);
        return result;
    }
};
```

---

# 🚀 Better Solution (Backtracking with Pruning)

### 💡 Key Idea

Instead of generating all combinations:

* Only add `'('` if we still have opening brackets left.
* Only add `')'` if it won’t break validity
  (`close < open`)

This avoids invalid generation.

---

### 🔹 Algorithm

1. Maintain:

   * `open` = number of '(' used
   * `close` = number of ')' used
2. Conditions:

   * If `open < n` → we can add '('
   * If `close < open` → we can add ')'
3. When string length = `2n`, add to result.

---

### ⏱ Time Complexity

* Number of valid combinations = **Catalan number**
* ≈ **O(4^n / √n)**

### 📦 Space Complexity

* **O(n)** recursion stack
* Output size = Catalan(n)

---

### 💻 C++ Code

```cpp id="opt1">
class Solution {
public:
    void backtrack(int open, int close, int n, string curr, vector<string>& result) {
        if(curr.length() == 2*n) {
            result.push_back(curr);
            return;
        }

        if(open < n)
            backtrack(open + 1, close, n, curr + "(", result);

        if(close < open)
            backtrack(open, close + 1, n, curr + ")", result);
    }

    vector<string> generateParenthesis(int n) {
        vector<string> result;
        backtrack(0, 0, n, "", result);
        return result;
    }
};
```

---

# ⚡ Alternative Optimal (Using String Reference for Efficiency)

Slight optimization:

* Pass string by reference
* Use push_back / pop_back
* Avoid string copying

---

### 💻 C++ Optimized Code

```cpp id="opt2">
class Solution {
public:
    void backtrack(int open, int close, int n, string &curr, vector<string>& result) {
        if(curr.length() == 2*n) {
            result.push_back(curr);
            return;
        }

        if(open < n) {
            curr.push_back('(');
            backtrack(open + 1, close, n, curr, result);
            curr.pop_back();
        }

        if(close < open) {
            curr.push_back(')');
            backtrack(open, close + 1, n, curr, result);
            curr.pop_back();
        }
    }

    vector<string> generateParenthesis(int n) {
        vector<string> result;
        string curr;
        backtrack(0, 0, n, curr, result);
        return result;
    }
};
```

---

# 🏆 Final Recommendation

| Approach                | Time              | Space | Recommendation |
| ----------------------- | ----------------- | ----- | -------------- |
| Generate All + Validate | O(n * 2^(2n))     | O(n)  | ❌ Inefficient  |
| Backtracking (Pruned)   | ⭐ **O(4^n / √n)** | O(n)  | ✅ Best         |

👉 In interviews, always use **backtracking with pruning**.
It generates only valid combinations and is the expected solution.

---
