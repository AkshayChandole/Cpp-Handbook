# [Two Sum](#two-sum)

Given an array of integers `nums` and an integer `target`, return **indices of the two numbers** such that they add up to `target`.

* Each input has **exactly one solution**
* You **may not use the same element twice**
* Return the answer in any order

---

## 🔎 Examples

**Example 1:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: nums[0] + nums[1] = 2 + 7 = 9
```

**Example 2:**

```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**

```
Input: nums = [3,3], target = 6
Output: [0,1]
```

---

# 🐢 Brute Force Solution

### 💡 Idea

Check every possible pair using two nested loops and see if their sum equals target.

### 🔹 Algorithm

1. Iterate `i` from `0` to `n-1`
2. For each `i`, iterate `j` from `i+1` to `n-1`
3. If `nums[i] + nums[j] == target`, return `{i, j}`

### ⏱ Time Complexity

* **O(n²)** — checking all pairs

### 📦 Space Complexity

* **O(1)** — no extra space used

### 💻 C++ Code

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < n; j++) {
                if(nums[i] + nums[j] == target) {
                    return {i, j};
                }
            }
        }
        return {};
    }
};
```

---

# 🚀 Better Solution (Using Sorting + Two Pointers)

### 💡 Idea

1. Store elements along with their indices.
2. Sort the array.
3. Use two pointers (left & right) to find the pair.

### 🔹 Algorithm

1. Create vector of `{value, index}`
2. Sort based on value
3. Initialize:

   * `left = 0`
   * `right = n-1`
4. While `left < right`:

   * If sum == target → return indices
   * If sum < target → left++
   * Else → right--

### ⏱ Time Complexity

* **O(n log n)** — sorting

### 📦 Space Complexity

* **O(n)** — storing value-index pairs

### 💻 C++ Code

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int n = nums.size();
        vector<pair<int,int>> arr;
        
        for(int i = 0; i < n; i++) {
            arr.push_back({nums[i], i});
        }
        
        sort(arr.begin(), arr.end());
        
        int left = 0, right = n - 1;
        
        while(left < right) {
            int sum = arr[left].first + arr[right].first;
            
            if(sum == target) {
                return {arr[left].second, arr[right].second};
            }
            else if(sum < target) {
                left++;
            }
            else {
                right--;
            }
        }
        
        return {};
    }
};
```

---

# ⚡ Optimum Solution (Using Hash Map)

### 💡 Idea

Use a hash map to store elements we have already seen.

For each element:

* Compute `complement = target - nums[i]`
* If complement exists in map → solution found
* Otherwise, store current element in map

### 🔹 Algorithm

1. Create `unordered_map<int, int>` (value → index)
2. Iterate through array:

   * Compute complement
   * If complement exists → return indices
   * Else insert current number with index

### ⏱ Time Complexity

* **O(n)** — single pass

### 📦 Space Complexity

* **O(n)** — hash map storage

### 💻 C++ Code

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> mp; // value -> index
        
        for(int i = 0; i < nums.size(); i++) {
            int complement = target - nums[i];
            
            if(mp.find(complement) != mp.end()) {
                return {mp[complement], i};
            }
            
            mp[nums[i]] = i;
        }
        
        return {};
    }
};
```

---

# 🏆 Final Recommendation

| Approach              | Time       | Space | When to Use        |
| --------------------- | ---------- | ----- | ------------------ |
| Brute Force           | O(n²)      | O(1)  | Small input        |
| Sorting + Two Pointer | O(n log n) | O(n)  | If sorting allowed |
| Hash Map              | **O(n)**   | O(n)  | ✅ Best approach    |

