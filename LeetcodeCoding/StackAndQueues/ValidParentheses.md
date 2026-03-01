# [Valid Parentheses](#valid-parentheses)



## 🟢 Valid Parentheses 

---

### 📌 Problem

Given a string `s` containing only:

```id="chars"
'('  ')'  '{'  '}'  '['  ']'
```

Return `true` if the string is valid.

A string is valid if:

1. Open brackets are closed by the same type.
2. Open brackets are closed in the correct order.
3. Every closing bracket has a corresponding opening bracket.

---

### 📌 Examples

```id="ex"
Input: "()"
Output: true

Input: "([)]"
Output: false

Input: "{[]}"
Output: true
```

---

# 🔴 1. Brute Force – Repeated String Replacement

### 💡 Idea

Keep removing valid pairs `"()"`, `"{}"`, `"[]"` until no more removals are possible.

If string becomes empty → valid.

---

### 💻 Code

```cpp
class Solution {
public:
    bool isValid(string s) {
        while(true) {
            int sizeBefore = s.size();

            size_t pos;
            while((pos = s.find("()")) != string::npos)
                s.erase(pos, 2);
            while((pos = s.find("{}")) != string::npos)
                s.erase(pos, 2);
            while((pos = s.find("[]")) != string::npos)
                s.erase(pos, 2);

            if(sizeBefore == s.size()) break;
        }
        return s.empty();
    }
};
```

### ⏱ Time Complexity: O(N²)

### 🗂 Space Complexity: O(1)

❌ Not efficient for large inputs.

---

# 🟠 2. Stack – Basic Implementation

### 💡 Idea

Use stack to track opening brackets.

* Push opening brackets
* On closing bracket:

  * If stack empty → invalid
  * If mismatch → invalid
  * Else pop

---

### 💻 Code

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;

        for(char c : s) {
            if(c == '(' || c == '{' || c == '[') {
                st.push(c);
            } else {
                if(st.empty()) return false;

                char top = st.top();
                st.pop();

                if((c == ')' && top != '(') ||
                   (c == '}' && top != '{') ||
                   (c == ']' && top != '['))
                    return false;
            }
        }
        return st.empty();
    }
};
```

### ⏱ Time Complexity: O(N)

### 🗂 Space Complexity: O(N)

✅ Standard interview solution.

---

# 🟢 3. Stack + HashMap (Cleaner & Extensible)

### 💡 Idea

Map closing brackets → corresponding opening brackets for cleaner checks.

---

### 💻 Code

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;

        unordered_map<char, char> mp = {
            {')', '('},
            {'}', '{'},
            {']', '['}
        };

        for(char c : s) {
            if(mp.count(c) == 0) {
                st.push(c);
            } else {
                if(st.empty() || st.top() != mp[c])
                    return false;
                st.pop();
            }
        }
        return st.empty();
    }
};
```

### ⏱ Time Complexity: O(N)

### 🗂 Space Complexity: O(N)

⭐ Cleanest and most scalable approach.

---

# 🏆 Final Comparison

| Approach    | Time  | Space | Recommended |
| ----------- | ----- | ----- | ----------- |
| Replacement | O(N²) | O(1)  | ❌           |
| Stack Basic | O(N)  | O(N)  | ✅           |
| Stack + Map | O(N)  | O(N)  | ⭐ Best      |

---

