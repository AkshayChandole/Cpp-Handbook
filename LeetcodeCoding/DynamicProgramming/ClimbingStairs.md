# [Climbing Stairs](#climbing-stairs)

## 🟢 Climbing Stairs

---

### 📌 Problem

You are climbing a staircase with `n` steps.

Each time you can climb:

* 1 step
* 2 steps

Return the number of distinct ways to reach the top.

---

### 📌 Examples

```id="ex1"
Input: n = 2
Output: 2
Explanation:
1+1
2
```

```id="ex2"
Input: n = 3
Output: 3
Explanation:
1+1+1
1+2
2+1
```

---

# 🔴 1. Brute Force (Pure Recursion)

### 💡 Idea

At step `n`, you can come from:

* `n-1`
* `n-2`

So:

[
f(n) = f(n-1) + f(n-2)
]

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int climbStairs(int n) {
        if(n <= 2) return n;
        return climbStairs(n - 1) + climbStairs(n - 2);
    }
};
```

---

### ⏱ Time Complexity

O(2^N)

### 🗂 Space Complexity

O(N) recursion stack

❌ Extremely slow (repeated calculations).

---

# 🟡 2. Better Solution (Memoization – Top Down DP)

### 💡 Idea

Store already computed values in DP array.

---

### 💻 Code (C++)

```cpp id="memo1"
class Solution {
public:
    int solve(int n, vector<int>& dp) {
        if(n <= 2) return n;
        if(dp[n] != -1) return dp[n];

        return dp[n] = solve(n - 1, dp) + solve(n - 2, dp);
    }

    int climbStairs(int n) {
        vector<int> dp(n + 1, -1);
        return solve(n, dp);
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(N)

✅ Much better.

---

# 🟢 3. Optimum Solution (Bottom-Up DP)

### 💡 Idea

Build solution iteratively.

---

### 💻 Code (C++)

```cpp id="dp1"
class Solution {
public:
    int climbStairs(int n) {
        if(n <= 2) return n;

        vector<int> dp(n + 1);
        dp[1] = 1;
        dp[2] = 2;

        for(int i = 3; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }

        return dp[n];
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(N)

---

# ⭐ 4. Most Optimal Solution (Space Optimized DP)

### 💡 Key Insight

We only need last two values.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    int climbStairs(int n) {
        if(n <= 2) return n;

        int prev2 = 1;  // f(1)
        int prev1 = 2;  // f(2)

        for(int i = 3; i <= n; i++) {
            int current = prev1 + prev2;
            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

⭐ Best solution for interviews.

---

# 🎯 Key Insight

This is basically the **Fibonacci sequence**:

[
f(n) = f(n-1) + f(n-2)
]

Sequence:

```id="fib"
n: 1  2  3  4  5  6
f: 1  2  3  5  8 13
```

---

# 🏆 Final Comparison

| Approach        | Time   | Space | Recommended |
| --------------- | ------ | ----- | ----------- |
| Recursion       | O(2^N) | O(N)  | ❌           |
| Memoization     | O(N)   | O(N)  | ✅           |
| Bottom-Up DP    | O(N)   | O(N)  | ✅           |
| Space Optimized | O(N)   | O(1)  | ⭐ Best      |

---

