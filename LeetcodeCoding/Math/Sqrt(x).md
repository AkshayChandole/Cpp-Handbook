
# [Sqrt(x)](#Sqrtx)

## 🟢 Sqrt(x)

---

### 📌 Problem

Given a non-negative integer `x`, compute and return:

```id="goal"
⌊ sqrt(x) ⌋
```

That is, return the **integer part** of the square root.

You must not use built-in exponent functions.

---

### 📌 Examples

```id="ex1"
Input: x = 4
Output: 2
```

```id="ex2"
Input: x = 8
Output: 2
Explanation:
sqrt(8) ≈ 2.828 → return 2
```

---

# 🔴 1. Brute Force (Linear Search)

### 💡 Idea

Start from 0:

* Increment i
* Stop when i*i > x
* Return i-1

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int mySqrt(int x) {
        long long i = 0;

        while(i * i <= x)
            i++;

        return i - 1;
    }
};
```

---

### ⏱ Time Complexity

O(√x)

### 🗂 Space Complexity

O(1)

❌ Too slow for large x.

---

# 🟡 2. Better Solution (Binary Search)

---

### 💡 Key Insight

Search in range:

```id="range"
[0, x]
```

Condition:

```id="condition"
mid * mid <= x
```

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    int mySqrt(int x) {
        if(x <= 1) return x;

        long long low = 1, high = x;
        long long ans = 0;

        while(low <= high) {
            long long mid = low + (high - low) / 2;

            if(mid * mid == x)
                return mid;
            else if(mid * mid < x) {
                ans = mid;
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }

        return ans;
    }
};
```

---

### ⏱ Time Complexity

O(log x)

### 🗂 Space Complexity

O(1)

✅ Most expected solution.

---

# 🟢 3. Optimum Solution (Newton’s Method)

---

### 💡 Mathematical Approach

Use Newton’s formula:

```id="formula"
r = (r + x/r) / 2
```

Repeat until convergence.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    int mySqrt(int x) {
        if(x == 0) return 0;

        long long r = x;

        while(r * r > x) {
            r = (r + x / r) / 2;
        }

        return r;
    }
};
```

---

### ⏱ Time Complexity

O(log x) (very fast convergence)

### 🗂 Space Complexity

O(1)

⭐ Fastest convergence in practice.

---

# 🔍 Why Use long long?

Because:

```id="overflow"
mid * mid can overflow int
```

Example:

```id="edge"
x = 2,147,483,647
```

Must use `long long`.

---

# 🔎 Dry Run (Binary Search)

Example:

```id="dry1"
x = 8
```

Search range:

```id="dry2"
1 to 8
```

Steps:

| mid | mid² | Action    |
| --- | ---- | --------- |
| 4   | 16   | too large |
| 2   | 4    | valid     |
| 3   | 9    | too large |

Answer = 2

---

# 🏆 Final Comparison

| Approach        | Time     | Space | Recommended |
| --------------- | -------- | ----- | ----------- |
| Linear Scan     | O(√x)    | O(1)  | ❌           |
| Binary Search   | O(log x) | O(1)  | ✅           |
| Newton’s Method | O(log x) | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Binary search on answer
* Overflow handling
* Mathematical optimization

Related problems:

* Pow(x, n)
* Valid Perfect Square
* Kth Smallest in Multiplication Table
* Find Peak Element

---

