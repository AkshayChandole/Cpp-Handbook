# [Plus One](#plus-one)

## 🟢 Plus One

---

### 📌 Problem

You are given a **non-empty array of digits** representing a non-negative integer.

The digits are stored such that:

* Most significant digit is at the front.
* Each element contains a single digit (0–9).

Add **one** to the integer and return the resulting array of digits.

---

### 📌 Examples

```id="ex1"
Input: digits = [1,2,3]
Output: [1,2,4]
Explanation: 123 + 1 = 124
```

```id="ex2"
Input: digits = [4,3,2,1]
Output: [4,3,2,2]
```

```id="ex3"
Input: digits = [9]
Output: [1,0]
```

```id="ex4"
Input: digits = [9,9,9]
Output: [1,0,0,0]
```

---

# 🔴 1. Brute Force (Convert to Number)

### 💡 Idea

1. Convert array to integer.
2. Add 1.
3. Convert back to array.

⚠ Problem: Overflow for large inputs.

---

### 💻 Code (C++ – Not Safe)

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        long long num = 0;

        for(int d : digits)
            num = num * 10 + d;

        num += 1;

        vector<int> result;
        while(num > 0) {
            result.push_back(num % 10);
            num /= 10;
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

O(N)

❌ Not valid for very large numbers.

---

# 🟡 2. Better Solution (Simulate Addition with Carry)

### 💡 Idea

Start from last digit:

* Add 1.
* If digit becomes 10 → set to 0 and carry 1.
* Continue left.
* If carry remains after full traversal → insert 1 at front.

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int n = digits.size();

        for(int i = n - 1; i >= 0; i--) {
            if(digits[i] < 9) {
                digits[i]++;
                return digits;
            }
            digits[i] = 0;
        }

        // If all digits were 9
        digits.insert(digits.begin(), 1);
        return digits;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1) (ignoring output)

✅ Standard interview solution.

---

# 🟢 3. Most Optimal (Cleaner Early Return Logic)

### 💡 Key Insight

As soon as a digit is not 9:

* Increment it.
* Return immediately.

Only if all digits are 9:

* Result becomes 1 followed by all zeros.

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        for(int i = digits.size() - 1; i >= 0; i--) {
            if(digits[i] != 9) {
                digits[i]++;
                return digits;
            }
            digits[i] = 0;
        }

        // All were 9
        digits.push_back(0);
        digits[0] = 1;
        return digits;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

⭐ Most efficient and clean solution.

---

# 🔍 Dry Run

### Case 1

```id="dry1"
[1,2,3]
```

Last digit ≠ 9 → increment → `[1,2,4]`

---

### Case 2

```id="dry2"
[1,2,9]
```

9 → set 0
Move left → 2 → increment
Result → `[1,3,0]`

---

### Case 3

```id="dry3"
[9,9,9]
```

All become 0 → `[0,0,0]`
Add leading 1 → `[1,0,0,0]`

---

# 🎯 Why This Works

This simulates elementary school addition:

* Start from rightmost digit.
* Propagate carry leftward.
* If carry survives entire array → increase digit count.

---

# 🏆 Final Comparison

| Approach           | Time | Space | Recommended |
| ------------------ | ---- | ----- | ----------- |
| Convert to Integer | O(N) | O(N)  | ❌           |
| Carry Simulation   | O(N) | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This tests:

* Edge case handling
* Carry propagation logic
* Thinking about large numbers without overflow

Related problems:

* Add Binary
* Add Two Numbers (Linked List)
* Multiply Strings

---
