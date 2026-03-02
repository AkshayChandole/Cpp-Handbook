# [Maximum Subarray](#Maximum-Subarray)

## 🟢 Maximum Subarray

---

### 📌 Problem

Given an integer array `nums`, find the **contiguous subarray** (containing at least one number) which has the **largest sum**, and return its sum.

---

### 📌 Examples

```id="ex1"
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6
```

```id="ex2"
Input: nums = [1]
Output: 1
```

```id="ex3"
Input: nums = [5,4,-1,7,8]
Output: 23
```

---

# 🔴 1. Brute Force (All Subarrays)

### 💡 Idea

* Generate all possible subarrays.
* Compute sum of each.
* Track maximum.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        int maxSum = INT_MIN;

        for(int i = 0; i < n; i++) {
            int sum = 0;
            for(int j = i; j < n; j++) {
                sum += nums[j];
                maxSum = max(maxSum, sum);
            }
        }

        return maxSum;
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(1)

❌ Too slow for large inputs.

---

# 🟡 2. Better Solution (Prefix Sum)

### 💡 Idea

Use prefix sums:

[
sum(i,j) = prefix[j] - prefix[i-1]
]

Still requires nested loops.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        vector<int> prefix(n);
        prefix[0] = nums[0];

        for(int i = 1; i < n; i++)
            prefix[i] = prefix[i - 1] + nums[i];

        int maxSum = nums[0];

        for(int i = 0; i < n; i++) {
            for(int j = i; j < n; j++) {
                int sum = prefix[j] - (i > 0 ? prefix[i - 1] : 0);
                maxSum = max(maxSum, sum);
            }
        }

        return maxSum;
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(N)

Still not optimal.

---

# 🟢 3. Optimum Solution (Kadane’s Algorithm)

### 💡 Key Insight

At each index:

Either:

* Start new subarray from current element
* Or extend previous subarray

So:

[
currentSum = max(nums[i], currentSum + nums[i])
]

Track global maximum.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int currentSum = nums[0];
        int maxSum = nums[0];

        for(int i = 1; i < nums.size(); i++) {
            currentSum = max(nums[i], currentSum + nums[i]);
            maxSum = max(maxSum, currentSum);
        }

        return maxSum;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

⭐ Most expected interview solution.

---

# 🔍 Intuition Behind Kadane

Example:

```id="dry1"
nums = [-2,1,-3,4,-1,2,1,-5,4]
```

We decide at each step:

* Is previous sum hurting me?
* If yes → start fresh.

---

### Dry Run

| i | num | currentSum | maxSum |
| - | --- | ---------- | ------ |
| 0 | -2  | -2         | -2     |
| 1 | 1   | 1          | 1      |
| 2 | -3  | -2         | 1      |
| 3 | 4   | 4          | 4      |
| 4 | -1  | 3          | 4      |
| 5 | 2   | 5          | 5      |
| 6 | 1   | 6          | 6      |
| 7 | -5  | 1          | 6      |
| 8 | 4   | 5          | 6      |

Answer = 6

---

# 🧠 Why It Works

If cumulative sum becomes negative:

It will reduce any future sum.

So discard it.

---

# 🔥 Edge Case: All Negative Numbers

Example:

```id="neg"
[-3,-2,-5]
```

Kadane still works because we initialize:

```cpp id="init"
currentSum = nums[0]
maxSum = nums[0]
```

Result = -2

---

# 🏆 Final Comparison

| Approach    | Time  | Space | Recommended |
| ----------- | ----- | ----- | ----------- |
| Brute Force | O(N²) | O(1)  | ❌           |
| Prefix Sum  | O(N²) | O(N)  | ❌           |
| Kadane      | O(N)  | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This is one of the most important DP patterns:

* Decide at each step:

  * Extend previous
  * Or start new

Related problems:

* Maximum Product Subarray
* Best Time to Buy and Sell Stock
* Circular Maximum Subarray
* 2D Maximum Submatrix Sum

---

