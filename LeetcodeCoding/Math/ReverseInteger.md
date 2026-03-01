
# [Reverse Integer](#reverse-integer)

## 🟢 Reverse Integer

### 📌 Problem

Given a **signed 32-bit integer `x`**, return `x` with its digits reversed.

If reversing `x` causes the value to go outside the **signed 32-bit integer range**
[
[-2^{31}, 2^{31} - 1]
]
then return `0`.

You must not use 64-bit integers to store the reversed value.

---

### 📌 Examples

**Example 1:**

```id="ex1"
Input: x = 123
Output: 321
```

**Example 2:**

```id="ex2"
Input: x = -123
Output: -321
```

**Example 3:**

```id="ex3"
Input: x = 120
Output: 21
```

**Example 4 (Overflow Case):**

```id="ex4"
Input: x = 1534236469
Output: 0
```

---

# 🔴 Brute Force Solution (Using String)

### 💡 Idea

1. Convert integer to string
2. Reverse the string
3. Convert back to integer
4. Check overflow

⚠ Problem: May internally use 64-bit conversion and is not ideal.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int reverse(int x) {
        string s = to_string(x);
        bool negative = false;

        if(s[0] == '-') {
            negative = true;
            s = s.substr(1);
        }

        std::reverse(s.begin(), s.end());

        long long num = stoll(s);
        if(negative) num = -num;

        if(num > INT_MAX || num < INT_MIN) return 0;

        return (int)num;
    }
};
```

---

### ⏱ Time Complexity

O(d)  (d = number of digits)

### 🗂 Space Complexity

O(d)

---

# 🟡 Better Solution (Mathematical Reversal)

### 💡 Idea

Extract digits using:

```
digit = x % 10
x /= 10
```

Build reversed number:

```
rev = rev * 10 + digit
```

⚠ Must handle overflow carefully.

---

# 🟢 Optimum Solution (Overflow Safe)

### 💡 Key Observation

Before updating:

```
rev = rev * 10 + digit
```

We must check overflow condition **before multiplying**.

---

### 🔹 Overflow Condition

For positive overflow:

```
rev > INT_MAX / 10
OR
rev == INT_MAX / 10 AND digit > 7
```

For negative overflow:

```
rev < INT_MIN / 10
OR
rev == INT_MIN / 10 AND digit < -8
```

Because:

* INT_MAX = 2147483647 (last digit = 7)
* INT_MIN = -2147483648 (last digit = -8)

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    int reverse(int x) {
        int rev = 0;

        while(x != 0) {
            int digit = x % 10;
            x /= 10;

            // Check overflow
            if(rev > INT_MAX / 10 || 
              (rev == INT_MAX / 10 && digit > 7)) 
                return 0;

            if(rev < INT_MIN / 10 || 
              (rev == INT_MIN / 10 && digit < -8)) 
                return 0;

            rev = rev * 10 + digit;
        }

        return rev;
    }
};
```

---

### ⏱ Time Complexity

O(d)

### 🗂 Space Complexity

O(1)

---

# 🎯 Why This Is Optimal

* No string conversion
* No 64-bit variable used
* Safe overflow detection
* Constant extra space

---

# 🏆 Final Comparison

| Approach      | Time | Space | Recommended |
| ------------- | ---- | ----- | ----------- |
| String Based  | O(d) | O(d)  | ❌           |
| Mathematical  | O(d) | O(1)  | ⚠           |
| Overflow Safe | O(d) | O(1)  | ✅           |

---
