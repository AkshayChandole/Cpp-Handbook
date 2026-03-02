# [Coin Change](#coin-change)

## 🟢 Coin Change

---

### 📌 Problem

You are given an integer array `coins` representing coins of different denominations and an integer `amount`.

Return the **fewest number of coins** needed to make up that amount.

If it is not possible to make that amount, return `-1`.

You may use each coin **unlimited times**.

---

### 📌 Examples

```id="ex1"
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

```id="ex2"
Input: coins = [2], amount = 3
Output: -1
```

```id="ex3"
Input: coins = [1], amount = 0
Output: 0
```

---

# 🔴 1. Brute Force (Pure Recursion)

### 💡 Idea

At each step:

* Try every coin.
* Recursively reduce amount.
* Return minimum coins.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int solve(vector<int>& coins, int amount) {
        if(amount == 0) return 0;
        if(amount < 0) return INT_MAX;

        int mini = INT_MAX;

        for(int coin : coins) {
            int res = solve(coins, amount - coin);
            if(res != INT_MAX)
                mini = min(mini, 1 + res);
        }

        return mini;
    }

    int coinChange(vector<int>& coins, int amount) {
        int ans = solve(coins, amount);
        return ans == INT_MAX ? -1 : ans;
    }
};
```

---

### ⏱ Time Complexity

O(coins^amount) → Exponential

### 🗂 Space Complexity

O(amount) recursion stack

❌ Too slow.

---

# 🟡 2. Better Solution (Memoization – Top Down DP)

### 💡 Idea

Avoid recomputation by storing results.

`dp[amount]` → minimum coins needed.

---

### 💻 Code (C++)

```cpp id="memo1"
class Solution {
public:
    int solve(vector<int>& coins, int amount, vector<int>& dp) {
        if(amount == 0) return 0;
        if(amount < 0) return INT_MAX;
        if(dp[amount] != -1) return dp[amount];

        int mini = INT_MAX;

        for(int coin : coins) {
            int res = solve(coins, amount - coin, dp);
            if(res != INT_MAX)
                mini = min(mini, 1 + res);
        }

        return dp[amount] = mini;
    }

    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, -1);
        int ans = solve(coins, amount, dp);
        return ans == INT_MAX ? -1 : ans;
    }
};
```

---

### ⏱ Time Complexity

O(N × amount)

### 🗂 Space Complexity

O(amount)

✅ Much better.

---

# 🟢 3. Optimum Solution (Bottom-Up DP)

### 💡 Key Insight

Let:

```
dp[i] = minimum coins needed to make amount i
```

Initialize:

```id="init"
dp[0] = 0
dp[i] = large number for i > 0
```

Transition:

```
dp[i] = min(dp[i], 1 + dp[i - coin]
```

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, amount + 1);

        dp[0] = 0;

        for(int i = 1; i <= amount; i++) {
            for(int coin : coins) {
                if(i - coin >= 0)
                    dp[i] = min(dp[i], dp[i - coin] + 1);
            }
        }

        return dp[amount] > amount ? -1 : dp[amount];
    }
};
```

---

### ⏱ Time Complexity

O(N × amount)

Where:

* N = number of coins

### 🗂 Space Complexity

O(amount)

⭐ Most expected interview solution.

---

# 🔍 Dry Run

Example:

```id="dry1"
coins = [1,2,5]
amount = 11
```

Build dp:

```id="dry2"
dp[0] = 0
dp[1] = 1
dp[2] = 1
dp[3] = 2
...
dp[11] = 3
```

Answer = 3

---

# 🎯 Why This Works

For each amount:

* Try all possible coins.
* Choose minimum among valid previous states.

This is classic **Unbounded Knapsack (Minimization)**.

---

# 🔥 Important Trick

Initialize dp with:

```cpp id="trick"
amount + 1
```

Because:

* Maximum coins needed cannot exceed `amount`.
* This acts as "infinity".

---

# 🏆 Final Comparison

| Approach     | Time          | Space     | Recommended |
| ------------ | ------------- | --------- | ----------- |
| Recursion    | Exponential   | O(amount) | ❌           |
| Memoization  | O(N × amount) | O(amount) | ✅           |
| Bottom-Up DP | O(N × amount) | O(amount) | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* DP on amounts
* Unbounded knapsack pattern
* Minimize operations

Related problems:

* Coin Change II (count combinations)
* Minimum Path Sum
* Perfect Squares
* Climbing Stairs (variation)

---

