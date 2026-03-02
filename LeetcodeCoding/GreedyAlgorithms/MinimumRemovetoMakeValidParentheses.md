# [Minimum Remove to Make Valid Parentheses](#minimum-remove-to-make-valid-parentheses)

## 🟢 Minimum Remove to Make Valid Parentheses

---

### 📌 Problem

Given a string `s` containing:

* lowercase letters
* `'('`
* `')'`

Remove the **minimum number of parentheses** so that the resulting string is valid.

Return the valid string.

A string is valid if:

* Every opening bracket has a corresponding closing bracket.
* Parentheses are properly matched in order.

---

### 📌 Examples

```id="ex1"
Input: s = "lee(t(c)o)de)"
Output: "lee(t(c)o)de"
```

```id="ex2"
Input: s = "a)b(c)d"
Output: "ab(c)d"
```

```id="ex3"
Input: s = "))(("
Output: ""
```

---

# 🔴 1. Brute Force (Try Removing All Combinations)

### 💡 Idea

* Try removing parentheses one by one.
* Check validity each time.
* Keep longest valid string.

⚠ Extremely expensive (exponential).

---

### ⏱ Time Complexity

O(2^N)

❌ Not practical.

---

# 🟡 2. Better Solution (Stack to Track Invalid Indices)

### 💡 Idea

1. Use stack to track indices of `'('`.
2. If we see `')'`:

   * If stack not empty → pop.
   * Else → mark index as invalid.
3. After traversal:

   * All remaining stack indices are invalid `'('`.
4. Remove all invalid indices.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    string minRemoveToMakeValid(string s) {
        stack<int> st;
        unordered_set<int> removeIndex;

        for(int i = 0; i < s.size(); i++) {
            if(s[i] == '(') {
                st.push(i);
            }
            else if(s[i] == ')') {
                if(!st.empty())
                    st.pop();
                else
                    removeIndex.insert(i);
            }
        }

        while(!st.empty()) {
            removeIndex.insert(st.top());
            st.pop();
        }

        string result;
        for(int i = 0; i < s.size(); i++) {
            if(!removeIndex.count(i))
                result += s[i];
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(N)

✅ Good solution.

---

# 🟢 3. Optimum Solution (Two-Pass Greedy – O(1) Extra Space)

### 💡 Key Insight

We don’t actually need stack.

### Pass 1 (Left → Right):

* Count open parentheses.
* If `')'` appears and no matching `'('` → skip it.

### Pass 2 (Right → Left):

* Remove extra `'('`.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    string minRemoveToMakeValid(string s) {
        string temp;
        int balance = 0;

        // First pass: remove extra ')'
        for(char c : s) {
            if(c == '(') {
                balance++;
            }
            else if(c == ')') {
                if(balance == 0)
                    continue;
                balance--;
            }
            temp += c;
        }

        string result;
        balance = 0;

        // Second pass: remove extra '('
        for(int i = temp.size() - 1; i >= 0; i--) {
            if(temp[i] == '(' && balance == 0)
                continue;

            if(temp[i] == ')')
                balance++;
            else if(temp[i] == '(')
                balance--;

            result += temp[i];
        }

        reverse(result.begin(), result.end());
        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1) (ignoring output)

⭐ Most optimal and clean approach.

---

# 🔍 Why Two Pass Works

Example:

```id="dry1"
s = "a)b(c)d"
```

Pass 1:

* Skip unmatched `')'`
* temp = `"ab(c)d"`

Pass 2:

* No extra `'('`
* Final result = `"ab(c)d"`

---

Example:

```id="dry2"
s = "))(("
```

Pass 1:

* Remove invalid `')'`
* temp = "(("

Pass 2:

* Remove extra `'('`
* result = ""

---

# 🎯 Why This Is Minimum Removal

Because:

* We only remove parentheses that are definitely invalid.
* We never remove a valid pair.
* Greedy removal guarantees minimal deletions.

---

# 🏆 Final Comparison

| Approach        | Time   | Space | Recommended |
| --------------- | ------ | ----- | ----------- |
| Brute Force     | O(2^N) | High  | ❌           |
| Stack           | O(N)   | O(N)  | ✅           |
| Two-Pass Greedy | O(N)   | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Parentheses validation
* Stack elimination logic
* Greedy string filtering
* Two-pass string techniques

Closely related to:

* Valid Parentheses
* Remove Invalid Parentheses (Hard)
* Longest Valid Parentheses

---
