# [Add Binary](#add-binary)

## 🟢 Add Binary

---

### 📌 Problem

Given two binary strings `a` and `b`, return their sum as a binary string.

You must:

* Perform binary addition manually.
* Not convert entire strings into integers (may overflow).

---

### 📌 Examples

```id="ex1"
Input: a = "11", b = "1"
Output: "100"
```

```id="ex2"
Input: a = "1010", b = "1011"
Output: "10101"
```

---

# 🔴 1. Brute Force (Convert to Integer)

---

### 💡 Idea

1. Convert binary strings to integers.
2. Add.
3. Convert back to binary.

---

### ❌ Why It Fails

* Large inputs (up to 10⁴ bits).
* Integer overflow.

---

### ⏱ Time Complexity

O(n)

But unsafe.

❌ Not acceptable.

---

# 🟡 2. Better Solution (Simulate Binary Addition)

---

### 💡 Key Idea

Binary addition rules:

| a | b | carry | sum | new carry |
| - | - | ----- | --- | --------- |
| 0 | 0 | 0     | 0   | 0         |
| 1 | 0 | 0     | 1   | 0         |
| 1 | 1 | 0     | 0   | 1         |
| 1 | 1 | 1     | 1   | 1         |

Algorithm:

* Start from last index of both strings.
* Add digits + carry.
* Append `sum % 2`.
* Update carry = `sum / 2`.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    string addBinary(string a, string b) {
        int i = a.size() - 1;
        int j = b.size() - 1;
        int carry = 0;

        string result = "";

        while(i >= 0 || j >= 0 || carry) {
            int sum = carry;

            if(i >= 0)
                sum += a[i--] - '0';
            if(j >= 0)
                sum += b[j--] - '0';

            result += (sum % 2) + '0';
            carry = sum / 2;
        }

        reverse(result.begin(), result.end());
        return result;
    }
};
```

---

### ⏱ Time Complexity

O(n)

### 🗂 Space Complexity

O(n)

✅ Most expected solution.

---

# 🟢 3. Optimum Insight (Without Reverse)

---

### 💡 Optimization

Instead of reversing at end:

* Insert at beginning (less efficient due to shifting)
* Or build using `push_back` and reverse (already optimal)
* Or use `stringstream` / reserve capacity

Best approach is already O(n).

Small improvement:

Reserve memory.

---

### 💻 Slightly Optimized Version

```cpp id="opt1"
class Solution {
public:
    string addBinary(string a, string b) {
        int i = a.size() - 1;
        int j = b.size() - 1;
        int carry = 0;

        string result;
        result.reserve(max(a.size(), b.size()) + 1);

        while(i >= 0 || j >= 0 || carry) {
            int sum = carry;

            if(i >= 0) sum += a[i--] - '0';
            if(j >= 0) sum += b[j--] - '0';

            result.push_back((sum % 2) + '0');
            carry = sum / 2;
        }

        reverse(result.begin(), result.end());
        return result;
    }
};
```

---

### ⏱ Time Complexity

O(n)

### 🗂 Space Complexity

O(n)

⭐ Optimal for constraints.

---

# 🔍 Dry Run

Example:

```id="dry1"
a = "1010"
b = "1011"
```

Step-by-step:

| a | b | carry | sum | result      |
| - | - | ----- | --- | ----------- |
| 0 | 1 | 0     | 1   | 1           |
| 1 | 1 | 0     | 2   | 0 (carry 1) |
| 0 | 0 | 1     | 1   | 1           |
| 1 | 1 | 0     | 2   | 0 (carry 1) |
| - | - | 1     | 1   | 1           |

Reverse → `"10101"`

---

# 🎯 Why This Works

Binary addition is identical to decimal addition except:

```id="binary"
Base = 2
Carry occurs at 2 instead of 10
```

---

# 🏆 Final Comparison

| Approach          | Time  | Space | Recommended |
| ----------------- | ----- | ----- | ----------- |
| Convert to Int    | Risky | O(1)  | ❌           |
| Manual Simulation | O(n)  | O(n)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* String manipulation
* Carry handling
* Simulation technique
* Edge cases (unequal lengths, carry at end)

Related problems:

* Add Strings
* Multiply Strings
* Add Two Numbers
* Plus One

---

