# [Subsets](#Subsets)

## 🟢 Subsets

---

### 📌 Problem

Given an integer array `nums` of **unique elements**, return **all possible subsets (the power set)**.

* The solution set must not contain duplicate subsets.
* You may return the answer in any order.

---

### 📌 Examples

**Example 1:**

```id="ex1"
Input: nums = [1,2,3]
Output:
[
 [],
 [1],
 [2],
 [3],
 [1,2],
 [1,3],
 [2,3],
 [1,2,3]
]
```

**Example 2:**

```id="ex2"
Input: nums = [0]
Output: [[], [0]]
```

---

# 🔴 1. Brute Force (Bit Manipulation)

### 💡 Idea

For an array of size `n`, total subsets = `2^n`.

Each number from `0` to `2^n - 1` represents a subset using binary representation.

If `jth` bit is set → include `nums[j]`.

---

### 🔹 Algorithm

1. Loop from `0` to `(1 << n) - 1`
2. For each number:

   * Check bits
   * Build subset

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> result;

        for(int mask = 0; mask < (1 << n); mask++) {
            vector<int> subset;

            for(int j = 0; j < n; j++) {
                if(mask & (1 << j)) {
                    subset.push_back(nums[j]);
                }
            }

            result.push_back(subset);
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N * 2^N)

### 🗂 Space Complexity

O(2^N)

---

# 🟡 2. Better Solution (Iterative Expansion)

### 💡 Idea

Start with empty subset.

For each number:

* Add it to all existing subsets.

Example:

```
Start: [[]]
Add 1 → [[], [1]]
Add 2 → [[], [1], [2], [1,2]]
Add 3 → ...
```

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> result = {{}};

        for(int num : nums) {
            int size = result.size();

            for(int i = 0; i < size; i++) {
                vector<int> temp = result[i];
                temp.push_back(num);
                result.push_back(temp);
            }
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N * 2^N)

### 🗂 Space Complexity

O(2^N)

---

# 🟢 3. Optimum Solution (Backtracking)

### 💡 Idea

Use recursion to decide:

* Include current element
* Exclude current element

This builds subset tree.

---

### 🔹 Algorithm

1. Start with index `0`
2. Add current subset to result
3. Loop from `index` to `n-1`

   * Include element
   * Recurse
   * Backtrack

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    vector<vector<int>> result;

    void backtrack(vector<int>& nums, int start, vector<int>& subset) {
        result.push_back(subset);

        for(int i = start; i < nums.size(); i++) {
            subset.push_back(nums[i]);
            backtrack(nums, i + 1, subset);
            subset.pop_back();  // backtrack
        }
    }

    vector<vector<int>> subsets(vector<int>& nums) {
        vector<int> subset;
        backtrack(nums, 0, subset);
        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N * 2^N)

### 🗂 Space Complexity

O(N) recursion stack
Output space = O(2^N)

---

# 🎯 Why Backtracking is Preferred in Interviews

* Natural way to generate combinations
* Easy to modify for:

  * Subsets II (with duplicates)
  * Combination Sum
  * Permutations
  * N-Queens

---

# 🏆 Final Comparison

| Approach     | Time     | Extra Space | Interview Preference |
| ------------ | -------- | ----------- | -------------------- |
| Bitmask      | O(N·2^N) | O(1)        | ✅                    |
| Iterative    | O(N·2^N) | O(1)        | ✅                    |
| Backtracking | O(N·2^N) | O(N)        | ⭐ Most Preferred     |

---


