# [Pow(x, n)](#powx-n)

## 🟢 Pow(x, n)

---

### 📌 Problem

Implement `pow(x, n)` which calculates:

[
x^n
]

Where:

* `x` is a double.
* `n` is a 32-bit signed integer.
* `n` may be negative.

Return the result as a double.

---

### 📌 Examples

```id="ex1"
Input: x = 2.00000, n = 10
Output: 1024.00000
```

```id="ex2"
Input: x = 2.10000, n = 3
Output: 9.26100
```

```id="ex3"
Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2^-2 = 1 / (2^2) = 1/4
```

---

# 🔴 1. Brute Force (Multiply n Times)

### 💡 Idea

Multiply `x` by itself `n` times.

Handle negative `n` by taking reciprocal.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    double myPow(double x, int n) {
        long long N = n;
        if(N < 0) {
            x = 1 / x;
            N = -N;
        }

        double result = 1.0;

        for(long long i = 0; i < N; i++) {
            result *= x;
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

❌ Too slow for large `n` (like 10⁹).

---

# 🟡 2. Better Solution (Recursive Fast Power)

### 💡 Key Insight

Instead of multiplying `n` times:

[
x^n =
\begin{cases}
(x^{n/2})^2 & \text{if n is even} \
x \cdot (x^{n/2})^2 & \text{if n is odd}
\end{cases}
]

This reduces exponent by half each time.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    double fastPow(double x, long long n) {
        if(n == 0) return 1;

        double half = fastPow(x, n / 2);

        if(n % 2 == 0)
            return half * half;
        else
            return half * half * x;
    }

    double myPow(double x, int n) {
        long long N = n;

        if(N < 0) {
            x = 1 / x;
            N = -N;
        }

        return fastPow(x, N);
    }
};
```

---

### ⏱ Time Complexity

O(log N)

### 🗂 Space Complexity

O(log N) recursion stack

✅ Much faster.

---

# 🟢 3. Optimum Solution (Iterative Binary Exponentiation)

### 💡 Even Better

Avoid recursion stack by using iterative approach.

Binary representation of exponent.

Example:

```id="bin"
n = 13 → binary = 1101
```

[
x^{13} = x^8 \cdot x^4 \cdot x^1
]

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    double myPow(double x, int n) {
        long long N = n;
        if(N < 0) {
            x = 1 / x;
            N = -N;
        }

        double result = 1.0;

        while(N > 0) {
            if(N % 2 == 1)
                result *= x;

            x *= x;
            N /= 2;
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(log N)

### 🗂 Space Complexity

O(1)

⭐ Most optimal solution.

---

# 🔍 Dry Run

Example:

```id="dry1"
x = 2, n = 13
```

Binary of 13:

```id="dry2"
1101
```

Process:

| N                   | result | x   |
| ------------------- | ------ | --- |
| 13                  | 1      | 2   |
| odd → result = 2    |        |     |
| 6                   | 2      | 4   |
| 3                   | 2      | 16  |
| odd → result = 32   |        |     |
| 1                   | 32     | 256 |
| odd → result = 8192 |        |     |

Final Answer = 8192

---

# 🎯 Important Edge Case

When:

```id="edge"
n = -2^31
```

We must convert to `long long` first.

Because:

```id="overflow"
abs(INT_MIN) overflows
```

So:

```cpp id="fix"
long long N = n;
```

is critical.

---

# 🏆 Final Comparison

| Approach             | Time     | Space    | Recommended |
| -------------------- | -------- | -------- | ----------- |
| Brute Force          | O(N)     | O(1)     | ❌           |
| Recursive Fast Power | O(log N) | O(log N) | ✅           |
| Iterative Binary Exp | O(log N) | O(1)     | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* Divide & Conquer
* Binary exponentiation
* Handling negative powers
* Avoiding overflow

Related problems:

* Modular Exponentiation
* Power of Two
* Square Root (Binary Search)
* Fast Fibonacci using matrix exponentiation

---

