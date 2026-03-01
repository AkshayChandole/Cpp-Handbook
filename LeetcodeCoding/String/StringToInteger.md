# [String To Integer](#string-to-integer)

## 🟢 String to Integer (atoi)

---

### 📌 Problem

Implement the `myAtoi(string s)` function that converts a string to a **32-bit signed integer**.

### Rules:

1. Ignore leading whitespace.
2. Optional `'+'` or `'-'` sign.
3. Read digits until non-digit character appears.
4. Clamp result to 32-bit integer range:

   * `INT_MAX = 2147483647`
   * `INT_MIN = -2147483648`

If no valid conversion → return `0`.

---

### 📌 Examples

```id="ex1"
Input: s = "42"
Output: 42
```

```id="ex2"
Input: s = "   -42"
Output: -42
```

```id="ex3"
Input: s = "4193 with words"
Output: 4193
```

```id="ex4"
Input: s = "words and 987"
Output: 0
```

```id="ex5"
Input: s = "-91283472332"
Output: -2147483648
```

---

# 🔴 1. Brute Force (Using long long)

### 💡 Idea

* Skip spaces
* Handle sign
* Convert digits using `long long`
* Clamp if overflow

⚠ Uses 64-bit integer (not ideal for strict overflow handling questions)

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int myAtoi(string s) {
        int i = 0, n = s.size();
        
        // 1. Skip spaces
        while(i < n && s[i] == ' ') i++;

        // 2. Handle sign
        int sign = 1;
        if(i < n && (s[i] == '+' || s[i] == '-')) {
            if(s[i] == '-') sign = -1;
            i++;
        }

        long long result = 0;

        // 3. Convert digits
        while(i < n && isdigit(s[i])) {
            result = result * 10 + (s[i] - '0');

            if(sign * result > INT_MAX) return INT_MAX;
            if(sign * result < INT_MIN) return INT_MIN;

            i++;
        }

        return (int)(sign * result);
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

---

# 🟡 2. Better Solution (Check Overflow Before Multiply)

### 💡 Idea

Instead of using `long long`, check overflow **before** multiplying:

Before:

```cpp
result = result * 10 + digit;
```

Check:

```cpp
if(result > INT_MAX/10 || 
   (result == INT_MAX/10 && digit > 7))
```

---

# 🟢 3. Optimum Solution (Fully Safe & Clean)

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    int myAtoi(string s) {
        int i = 0, n = s.size();

        // 1. Skip leading spaces
        while(i < n && s[i] == ' ')
            i++;

        // 2. Handle sign
        int sign = 1;
        if(i < n && (s[i] == '+' || s[i] == '-')) {
            if(s[i] == '-') sign = -1;
            i++;
        }

        int result = 0;

        // 3. Process digits
        while(i < n && isdigit(s[i])) {
            int digit = s[i] - '0';

            // Overflow check
            if(result > INT_MAX / 10 ||
               (result == INT_MAX / 10 && digit > 7)) {
                return sign == 1 ? INT_MAX : INT_MIN;
            }

            result = result * 10 + digit;
            i++;
        }

        return sign * result;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

⭐ Most interview-preferred solution.

---

# 🔍 Why Digit > 7?

Because:

```
INT_MAX = 2147483647
```

Last digit is `7`.

If:

```
result == 214748364
```

Then next digit must be ≤ 7.

If digit > 7 → overflow.

For negative:

```
INT_MIN = -2147483648
```

Handled automatically via sign logic.

---

# 🎯 Edge Cases Covered

| Input        | Output     |
| ------------ | ---------- |
| ""           | 0          |
| "   "        | 0          |
| "+"          | 0          |
| "-abc"       | 0          |
| "000123"     | 123        |
| "+-12"       | 0          |
| "2147483648" | 2147483647 |

---

# 🏆 Final Comparison

| Approach           | Time | Space | Recommended |
| ------------------ | ---- | ----- | ----------- |
| Using long long    | O(N) | O(1)  | ❌           |
| Pre-overflow check | O(N) | O(1)  | ⭐ Best      |


---
