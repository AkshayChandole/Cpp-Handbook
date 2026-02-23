
# [Balanced Paranthesis](#Balanced-Paranthesis)


## 📌 Problem Definition

Given a string `s` containing only:

```text
'(', ')', '{', '}', '[' , ']'
```

Determine if the input string is **valid (balanced)**.

---

## 📌 Rules

A string is valid if:

1. Open brackets must be closed by the same type.
2. Open brackets must be closed in correct order.
3. Every closing bracket must have a matching opening bracket.

---

## 📌 Example

#### Example 1

```
Input: "()"
Output: true
```

#### Example 2

```
Input: "()[]{}"
Output: true
```

#### Example 3

```
Input: "(]"
Output: false
```

#### Example 4

```
Input: "([)]"
Output: false
```

---

## 🔥 Core Idea

This is a classic **Stack problem**.

Why?

Because:

```
Last opened bracket must be closed first
```

This is exactly:

```
LIFO → Stack behavior
```

---

## ✅ Algorithm

---

### Step-by-step Logic

1. Create empty stack.
2. Traverse string:

   * If opening bracket → push into stack.
   * If closing bracket:

     * If stack empty → invalid.
     * Check if top matches.
     * If not → invalid.
     * Else → pop.
3. After traversal:

   * If stack empty → valid.
   * Else → invalid.

---

## 💻 C++ Implementation

---

```cpp
#include <iostream>
#include <stack>
using namespace std;

class Solution {
public:
    bool isValid(string s) {
        stack<char> st;

        for (char ch : s) {

            // If opening bracket
            if (ch == '(' || ch == '{' || ch == '[') {
                st.push(ch);
            }
            else {
                // If closing bracket and stack empty
                if (st.empty())
                    return false;

                char top = st.top();
                st.pop();

                if ((ch == ')' && top != '(') ||
                    (ch == '}' && top != '{') ||
                    (ch == ']' && top != '[')) {
                    return false;
                }
            }
        }

        return st.empty();
    }
};

int main() {

    Solution sol;

    cout << sol.isValid("()[]{}") << endl;  // 1 (true)
    cout << sol.isValid("([)]") << endl;    // 0 (false)

    return 0;
}
```

---

## ⏱ Time Complexity

```
O(n)
```

Each character processed once.

---

## 🧠 Space Complexity

```
O(n)
```

Worst case: all opening brackets.

---

## 🔥 Why This Works

Example:

Input:

```
([{}])
```

Stack operations:

```
Push '('
Push '['
Push '{'
Pop '{'
Pop '['
Pop '('
```

Stack empty → valid.

---

## 🔥 Common Interview Edge Cases

| Input  | Result |
| ------ | ------ |
| ""     | true   |
| "("    | false  |
| ")"    | false  |
| "((("  | false  |
| "{[]}" | true   |

---

## 🔥 Optimized Version Using Map

Cleaner matching:

```cpp
#include <iostream>
#include <stack>
#include <unordered_map>
using namespace std;

bool isValid(string s) {

    stack<char> st;
    unordered_map<char, char> mp = {
        {')','('},
        {'}','{'},
        {']','['}
    };

    for (char ch : s) {

        if (mp.count(ch) == 0) {
            st.push(ch);  // opening bracket
        }
        else {
            if (st.empty() || st.top() != mp[ch])
                return false;

            st.pop();
        }
    }

    return st.empty();
}
```

---

## 🔥 Interview Discussion Points

You should mention:

* Why stack is ideal
* Why recursion is not good here
* Early return for invalid cases
* Handling edge cases
* Can be extended for:

  * HTML tags
  * Expression parsing
  * Compiler syntax validation

---

## 🎯 Final Summary

| Concept        | Explanation    |
| -------------- | -------------- |
| Data Structure | Stack          |
| Time           | O(n)           |
| Space          | O(n)           |
| Pattern        | Matching Pairs |

---
