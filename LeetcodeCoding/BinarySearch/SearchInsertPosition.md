# [Search Insert Position](#Search-Insert-Position)

## 🟢 Search Insert Position

---

### 📌 Problem

Given a **sorted array of distinct integers** and a `target` value, return:

* The index if the target is found.
* Otherwise, the index where it would be inserted in order.

You must write an algorithm with **O(log N)** time complexity.

---

### 📌 Examples

```id="ex1"
Input: nums = [1,3,5,6], target = 5
Output: 2
```

```id="ex2"
Input: nums = [1,3,5,6], target = 2
Output: 1
```

```id="ex3"
Input: nums = [1,3,5,6], target = 7
Output: 4
```

```id="ex4"
Input: nums = [1,3,5,6], target = 0
Output: 0
```

---

# 🔴 1. Brute Force (Linear Scan)

### 💡 Idea

Traverse array:

* If element ≥ target → return index.
* If end reached → return array size.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        for(int i = 0; i < nums.size(); i++) {
            if(nums[i] >= target)
                return i;
        }
        return nums.size();
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

❌ Not acceptable (must be O(log N)).

---

# 🟡 2. Better Solution (Binary Search – Classic)

### 💡 Idea

Use binary search.

If not found, `low` will end up at correct insert position.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int low = 0;
        int high = nums.size() - 1;

        while(low <= high) {
            int mid = low + (high - low) / 2;

            if(nums[mid] == target)
                return mid;
            else if(nums[mid] < target)
                low = mid + 1;
            else
                high = mid - 1;
        }

        return low;
    }
};
```

---

### ⏱ Time Complexity

O(log N)

### 🗂 Space Complexity

O(1)

✅ Standard solution.

---

# 🟢 3. Optimum Interpretation (Lower Bound Concept)

### 💡 Key Insight

This problem is essentially:

> Find the **first index where element ≥ target**

This is exactly **lower_bound**.

---

### 💻 Code Using STL

```cpp id="opt1"
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        return lower_bound(nums.begin(), nums.end(), target) - nums.begin();
    }
};
```

---

### ⏱ Time Complexity

O(log N)

### 🗂 Space Complexity

O(1)

⭐ Cleanest solution.

---

# 🔍 Why Returning `low` Works?

After loop:

```cpp
while(low <= high)
```

If target not found:

* `high` < `low`
* `low` is smallest index where element should be inserted

Example:

```id="dry1"
nums = [1,3,5,6]
target = 2
```

Binary search ends with:

```id="dry2"
low = 1
high = 0
```

Insert at index 1.

---

# 🎯 Edge Cases

| Case                | Result            |
| ------------------- | ----------------- |
| Insert at beginning | return 0          |
| Insert at end       | return n          |
| Exact match         | return index      |
| Single element      | handled correctly |

---

# 🏆 Final Comparison

| Approach        | Time     | Space | Recommended |
| --------------- | -------- | ----- | ----------- |
| Linear Scan     | O(N)     | O(1)  | ❌           |
| Binary Search   | O(log N) | O(1)  | ✅           |
| STL lower_bound | O(log N) | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Binary search fundamentals
* Lower bound concept
* Edge case handling

Closely related to:

* First Bad Version
* Find First & Last Position
* Search in Rotated Sorted Array
* Lower Bound / Upper Bound

---

