# [Contains Duplicate II](#contains-duplicate-ii)


## 🟢 Contains Duplicate II

---

### 📌 Problem

Given an integer array `nums` and an integer `k`, return:

* `true` if there exist two **distinct indices** `i` and `j` such that:

  * `nums[i] == nums[j]`
  * `|i - j| ≤ k`

* Otherwise, return `false`.

---

### 📌 Examples

```id="ex1"
Input: nums = [1,2,3,1], k = 3
Output: true
Explanation:
nums[0] = nums[3] = 1
|0 - 3| = 3 ≤ 3
```

```id="ex2"
Input: nums = [1,0,1,1], k = 1
Output: true
```

```id="ex3"
Input: nums = [1,2,3,1,2,3], k = 2
Output: false
```

---

# 🔴 1. Brute Force (Check All Pairs)

### 💡 Idea

Check every pair `(i, j)`:

* If `nums[i] == nums[j]`
* And `|i - j| ≤ k`
* Return true

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        int n = nums.size();

        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < n; j++) {
                if(nums[i] == nums[j] && abs(i - j) <= k)
                    return true;
            }
        }

        return false;
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(1)

❌ Too slow.

---

# 🟡 2. Better Solution (HashMap to Track Last Index)

### 💡 Idea

Use:

```cpp
unordered_map<int, int> mp;
```

Store:

```
value → last index seen
```

For each element:

* If seen before:

  * Check distance.
* Update index.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int, int> mp;

        for(int i = 0; i < nums.size(); i++) {
            if(mp.count(nums[i])) {
                if(i - mp[nums[i]] <= k)
                    return true;
            }
            mp[nums[i]] = i;
        }

        return false;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(N)

✅ Efficient and simple.

---

# 🟢 3. Optimum Solution (Sliding Window + HashSet)

### 💡 Even Cleaner Idea

Maintain a window of size at most `k`.

Use:

```cpp
unordered_set<int> window;
```

At index `i`:

* If element already in set → return true.
* Insert current element.
* If window size > k → remove element at `i-k`.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_set<int> window;

        for(int i = 0; i < nums.size(); i++) {
            if(window.count(nums[i]))
                return true;

            window.insert(nums[i]);

            if(window.size() > k)
                window.erase(nums[i - k]);
        }

        return false;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(min(N, k))

⭐ Most optimal in practice.

---

# 🔍 Why Sliding Window Works

We only care about elements within distance `k`.

So at index `i`, we only need to check previous `k` elements.

Window always contains:

```
[i-k, i-1]
```

---

# 🔎 Dry Run

Example:

```id="dry1"
nums = [1,2,3,1]
k = 3
```

Step-by-step:

| i | window                     | action |
| - | -------------------------- | ------ |
| 0 | {1}                        |        |
| 1 | {1,2}                      |        |
| 2 | {1,2,3}                    |        |
| 3 | 1 already in window → true |        |

---

Example 2:

```id="dry2"
nums = [1,2,3,1,2,3]
k = 2
```

Window always size 2 → duplicates too far apart → false.

---

# 🏆 Final Comparison

| Approach       | Time  | Space | Recommended |
| -------------- | ----- | ----- | ----------- |
| Brute Force    | O(N²) | O(1)  | ❌           |
| HashMap        | O(N)  | O(N)  | ✅           |
| Sliding Window | O(N)  | O(K)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* Hashing
* Sliding window technique
* Window size constraint logic

Related problems:

* Contains Duplicate
* Contains Duplicate III
* Longest Substring Without Repeating Characters
* Sliding Window Maximum

---
