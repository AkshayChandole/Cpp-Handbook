# [Three Sum](#three-sum)

## 🟢 3Sum

---

### 📌 Problem

Given an integer array `nums`, return **all unique triplets**
`[nums[i], nums[j], nums[k]]` such that:

[
nums[i] + nums[j] + nums[k] = 0
]

Constraints:

* `i ≠ j ≠ k`
* Triplets must be unique (no duplicates)

---

### 📌 Example

```id="ex1"
Input: nums = [-1,0,1,2,-1,-4]

Output:
[
 [-1,-1,2],
 [-1,0,1]
]
```

---

# 🔴 1. Brute Force (Three Nested Loops)

### 💡 Idea

Try every combination of three numbers.

Check if sum == 0.

Use set to avoid duplicates.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        int n = nums.size();
        set<vector<int>> st;

        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < n; j++) {
                for(int k = j + 1; k < n; k++) {
                    if(nums[i] + nums[j] + nums[k] == 0) {
                        vector<int> temp = {nums[i], nums[j], nums[k]};
                        sort(temp.begin(), temp.end());
                        st.insert(temp);
                    }
                }
            }
        }

        return vector<vector<int>>(st.begin(), st.end());
    }
};
```

---

### ⏱ Time Complexity

O(N³)

### 🗂 Space Complexity

O(N)

❌ Too slow.

---

# 🟡 2. Better Solution (Sorting + HashSet)

### 💡 Idea

1. Sort array.
2. Fix one element.
3. Use HashSet to find complement.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        set<vector<int>> result;

        for(int i = 0; i < n; i++) {
            unordered_set<int> seen;

            for(int j = i + 1; j < n; j++) {
                int complement = -nums[i] - nums[j];

                if(seen.count(complement)) {
                    result.insert({nums[i], complement, nums[j]});
                }

                seen.insert(nums[j]);
            }
        }

        return vector<vector<int>>(result.begin(), result.end());
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(N)

Better, but still uses extra set for uniqueness.

---

# 🟢 3. Optimum Solution (Sorting + Two Pointers)

### 💡 Key Insight

After sorting:

* Fix first element `i`
* Use two pointers `left` and `right`
* Move pointers based on sum
* Skip duplicates

---

### 🔹 Algorithm

1. Sort array
2. For each index `i`:

   * Skip duplicates
   * Set `left = i+1`, `right = n-1`
   * While `left < right`:

     * Compute sum
     * If sum == 0:

       * Add triplet
       * Skip duplicates
     * If sum < 0 → move left
     * If sum > 0 → move right

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> result;
        int n = nums.size();

        for(int i = 0; i < n - 2; i++) {
            if(i > 0 && nums[i] == nums[i - 1])
                continue; // skip duplicates

            int left = i + 1;
            int right = n - 1;

            while(left < right) {
                int sum = nums[i] + nums[left] + nums[right];

                if(sum == 0) {
                    result.push_back({nums[i], nums[left], nums[right]});

                    while(left < right && nums[left] == nums[left + 1])
                        left++;
                    while(left < right && nums[right] == nums[right - 1])
                        right--;

                    left++;
                    right--;
                }
                else if(sum < 0)
                    left++;
                else
                    right--;
            }
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(1) (excluding output)

⭐ Most expected interview solution.

---

# 🔍 Why Sorting Is Important

Sorting helps:

* Easily skip duplicates
* Use two-pointer technique
* Reduce complexity from O(N³) → O(N²)

---

# 🔥 Duplicate Handling

Two critical checks:

```cpp
if(i > 0 && nums[i] == nums[i - 1]) continue;
```

And inside while loop:

```cpp
while(nums[left] == nums[left + 1]) left++;
while(nums[right] == nums[right - 1]) right--;
```

Without these → duplicate triplets.

---

# 🏆 Final Comparison

| Approach     | Time  | Space | Recommended |
| ------------ | ----- | ----- | ----------- |
| Triple Loop  | O(N³) | O(N)  | ❌           |
| HashSet      | O(N²) | O(N)  | ⚠           |
| Two Pointers | O(N²) | O(1)  | ⭐ Best      |

---

O(N²) is optimal
* Visual dry run explanation 🚀
