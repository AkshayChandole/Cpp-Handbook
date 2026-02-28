# [Best Time to Buy and Sell Stock](#best-time-to-buy-and-sell-stock)

## 🟢 Best Time to Buy and Sell Stock

---

### 📌 Problem

Given an array `prices` where `prices[i]` is the price of a given stock on the `i-th` day, you want to maximize your profit by choosing **one day to buy** and **a different day in the future to sell**.

Return the maximum profit you can achieve. If no profit is possible, return `0`.

---

### 📌 Examples

**Example 1:**

```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6 - 1 = 5.
```

**Example 2:**

```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: No transaction is done since price keeps decreasing.
```

---

# 🔴 Brute Force Solution

### 💡 Idea

Check every possible pair `(i, j)` such that `j > i` and calculate `prices[j] - prices[i]`.
Return the maximum difference.

---

### 🔹 Algorithm

1. Initialize `maxProfit = 0`
2. For every `i` from `0` to `n-1`
3. For every `j` from `i+1` to `n-1`
4. Compute profit = `prices[j] - prices[i]`
5. Update `maxProfit`

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int maxProfit = 0;

        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < n; j++) {
                maxProfit = max(maxProfit, prices[j] - prices[i]);
            }
        }

        return maxProfit;
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(1)

---

# 🟡 Better Solution (Using Extra Array)

### 💡 Idea

We track the **minimum price so far** from the left.

Instead of checking all pairs, we:

* Keep updating minimum price
* Calculate profit at each step

---

### 🔹 Algorithm

1. Initialize `minPrice = INT_MAX`
2. Initialize `maxProfit = 0`
3. Traverse the array:

   * Update `minPrice`
   * Calculate `profit = price - minPrice`
   * Update `maxProfit`

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int minPrice = INT_MAX;
        int maxProfit = 0;

        for(int price : prices) {
            minPrice = min(minPrice, price);
            maxProfit = max(maxProfit, price - minPrice);
        }

        return maxProfit;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

---

# 🟢 Optimum Solution (Kadane’s Algorithm Concept)

### 💡 Key Insight

This problem can also be viewed as:

Find maximum subarray sum of daily differences.

Create a difference array:

```
diff[i] = prices[i] - prices[i-1]
```

Then find maximum subarray sum using Kadane’s Algorithm.

---

### 🔹 Algorithm

1. Traverse from index 1
2. Compute daily difference
3. Apply Kadane's logic:

   * `currentProfit = max(0, currentProfit + diff)`
   * Update `maxProfit`

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxProfit = 0;
        int currentProfit = 0;

        for(int i = 1; i < prices.size(); i++) {
            int diff = prices[i] - prices[i - 1];
            currentProfit = max(0, currentProfit + diff);
            maxProfit = max(maxProfit, currentProfit);
        }

        return maxProfit;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

---

# 🎯 Final Takeaway

| Approach    | Time Complexity | Space Complexity |
| ----------- | --------------- | ---------------- |
| Brute Force | O(N²)           | O(1)             |
| Better      | O(N)            | O(1)             |
| Optimum     | O(N)            | O(1)             |

👉 The **minimum-price tracking approach** is the cleanest and most commonly asked in interviews.

---

