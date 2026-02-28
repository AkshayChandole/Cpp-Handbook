# [Subarray Sum Equals K](#subarray-sum-equals-k)

## 🟢 Subarray Sum Equals K

---

### 📌 Problem

Given an integer array `nums` and an integer `k`, return the **total number of continuous subarrays whose sum equals to `k`**.

---

### 📌 Examples

**Example 1:**

```id="ex1"
Input: nums = [1,1,1], k = 2
Output: 2
Explanation: Subarrays → [1,1] (index 0-1) and [1,1] (index 1-2)
```

**Example 2:**

```id="ex2"
Input: nums = [1,2,3], k = 3
Output: 2
Explanation: Subarrays → [1,2] and [3]
```

---

# 🔴 Brute Force Solution (Three Loops)

### 💡 Idea

Generate all possible subarrays and calculate their sum individually.

---

### 🔹 Algorithm

1. Iterate `i` from `0 → n-1`
2. Iterate `j` from `i → n-1`
3. Compute sum from `i → j`
4. If sum == k → increment count

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int n = nums.size();
        int count = 0;

        for(int i = 0; i < n; i++) {
            for(int j = i; j < n; j++) {
                int sum = 0;
                for(int l = i; l <= j; l++) {
                    sum += nums[l];
                }
                if(sum == k) count++;
            }
        }

        return count;
    }
};
```

---

### ⏱ Time Complexity

O(N³)

### 🗂 Space Complexity

O(1)

---

# 🟡 Better Solution (Two Loops)

### 💡 Idea

Instead of recalculating sum every time, keep a running sum inside inner loop.

---

### 🔹 Algorithm

1. For each starting index `i`
2. Keep a running `sum`
3. Extend subarray one by one
4. If `sum == k`, increment count

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int n = nums.size();
        int count = 0;

        for(int i = 0; i < n; i++) {
            int sum = 0;
            for(int j = i; j < n; j++) {
                sum += nums[j];
                if(sum == k) count++;
            }
        }

        return count;
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(1)

---

# 🟢 Optimum Solution (Prefix Sum + HashMap)

### 💡 Key Insight

If:

```
prefixSum[j] - prefixSum[i] = k
```

Then:

```
prefixSum[i] = prefixSum[j] - k
```

So for every prefix sum, we check how many times
`(currentSum - k)` has occurred before.

---

### 🔹 Algorithm

1. Use unordered_map to store frequency of prefix sums
2. Initialize map with `{0 : 1}` (important for exact match cases)
3. Traverse array:

   * Add current number to `currentSum`
   * Check if `(currentSum - k)` exists in map
   * Add its frequency to count
   * Increase frequency of `currentSum`

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> mp;
        mp[0] = 1;   // base case

        int currentSum = 0;
        int count = 0;

        for(int num : nums) {
            currentSum += num;

            if(mp.find(currentSum - k) != mp.end()) {
                count += mp[currentSum - k];
            }

            mp[currentSum]++;
        }

        return count;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(N)

---

# 🎯 Why Sliding Window Doesn't Work?

Sliding window works only when:

* All numbers are positive

Here, array may contain:

* Negative numbers
* Zero

So window size cannot be adjusted deterministically.

---

# 🏆 Final Comparison

| Approach                   | Time  | Space |
| -------------------------- | ----- | ----- |
| Brute Force                | O(N³) | O(1)  |
| Better                     | O(N²) | O(1)  |
| Optimal (Prefix + HashMap) | O(N)  | O(N)  |

---
