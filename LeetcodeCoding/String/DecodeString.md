# [Decode String](#Decode-String)

## 🟢 Decode String

---

### 📌 Problem

Given an encoded string, return its decoded version.

Encoding rule:

```id="rule"
k[encoded_string]
```

Meaning:

* The `encoded_string` inside brackets is repeated exactly `k` times.
* `k` is guaranteed to be positive.
* The string may contain nested patterns.

---

### 📌 Examples

```id="ex1"
Input: s = "3[a]2[bc]"
Output: "aaabcbc"
```

```id="ex2"
Input: s = "3[a2[c]]"
Output: "accaccacc"
```

```id="ex3"
Input: s = "2[abc]3[cd]ef"
Output: "abcabccdcdcdef"
```

---

# 🔴 1. Brute Force (Recursive Parsing Without Stack)

### 💡 Idea

Recursively parse:

* When digit found → read full number.
* Recursively decode substring inside brackets.

Works but tricky for nested cases.

---

### 💻 Code (C++ – Recursive)

```cpp id="bf1"
class Solution {
public:
    string decodeString(string s, int& i) {
        string result;

        while(i < s.size() && s[i] != ']') {
            if(isdigit(s[i])) {
                int k = 0;
                while(isdigit(s[i]))
                    k = k * 10 + (s[i++] - '0');

                i++; // skip '['
                string decoded = decodeString(s, i);
                i++; // skip ']'

                while(k--)
                    result += decoded;
            } else {
                result += s[i++];
            }
        }

        return result;
    }

    string decodeString(string s) {
        int i = 0;
        return decodeString(s, i);
    }
};
```

---

### ⏱ Time Complexity

O(N × max_repeat)

### 🗂 Space Complexity

O(N) recursion stack

✅ Works well.

---

# 🟡 2. Better Solution (Two Stacks)

### 💡 Idea

Maintain:

* `countStack`
* `stringStack`
* `currentString`
* `currentNumber`

When:

* Digit → build number.
* '[' → push current state.
* ']' → pop and build repeated string.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    string decodeString(string s) {
        stack<int> countStack;
        stack<string> stringStack;

        string currentString = "";
        int currentNumber = 0;

        for(char c : s) {
            if(isdigit(c)) {
                currentNumber = currentNumber * 10 + (c - '0');
            }
            else if(c == '[') {
                countStack.push(currentNumber);
                stringStack.push(currentString);

                currentNumber = 0;
                currentString = "";
            }
            else if(c == ']') {
                int repeat = countStack.top();
                countStack.pop();

                string temp = currentString;

                currentString = stringStack.top();
                stringStack.pop();

                while(repeat--)
                    currentString += temp;
            }
            else {
                currentString += c;
            }
        }

        return currentString;
    }
};
```

---

### ⏱ Time Complexity

O(N × max_repeat)

### 🗂 Space Complexity

O(N)

✅ Most common interview solution.

---

# 🟢 3. Optimum Solution (Single Stack of Strings)

### 💡 Even Cleaner

Use single stack of strings.

Push everything:

* Numbers
* Strings
* Brackets

When encountering `]`:

* Pop until `[`
* Build string
* Multiply
* Push back

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    string decodeString(string s) {
        stack<string> st;

        for(char c : s) {
            if(c != ']') {
                st.push(string(1, c));
            } else {
                string curr = "";

                while(!st.empty() && st.top() != "[") {
                    curr = st.top() + curr;
                    st.pop();
                }

                st.pop(); // remove '['

                string num = "";
                while(!st.empty() && isdigit(st.top()[0])) {
                    num = st.top() + num;
                    st.pop();
                }

                int k = stoi(num);
                string repeated = "";

                while(k--)
                    repeated += curr;

                st.push(repeated);
            }
        }

        string result = "";

        while(!st.empty()) {
            result = st.top() + result;
            st.pop();
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N × max_repeat)

### 🗂 Space Complexity

O(N)

⭐ Clean stack-based approach.

---

# 🔍 Dry Run

Example:

```id="dry1"
3[a2[c]]
```

Process:

```
3[
  a
  2[
     c
   ]
]
```

Inner:

```
2[c] → cc
```

Outer:

```
3[acc] → accaccacc
```

---

# 🎯 Why Stack Works

Because:

* Each `[` marks new context.
* When `]` found:

  * We complete inner substring.
  * Multiply.
  * Attach to outer context.

Similar to:

* Valid Parentheses
* Basic Calculator
* Expression evaluation

---

# 🏆 Final Comparison

| Approach     | Time | Space | Recommended |
| ------------ | ---- | ----- | ----------- |
| Recursive    | O(N) | O(N)  | ✅           |
| Two Stacks   | O(N) | O(N)  | ⭐ Best      |
| Single Stack | O(N) | O(N)  | ⭐ Cleanest  |

---

# 🎯 Interview Insight

This problem tests:

* Stack usage
* Parsing logic
* Handling nested structures
* Multi-digit numbers

Related problems:

* Basic Calculator
* Valid Parentheses
* Remove Adjacent Duplicates
* Mini Parser

---

