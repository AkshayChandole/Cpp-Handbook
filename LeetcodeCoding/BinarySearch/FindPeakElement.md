# [Find Peak Element](#find-peak-element)

## 🟢 Find Peak Element

---

### 📌 Problem

Given an integer array `nums`, find a **peak element**, and return its index.

A peak element is an element that is **strictly greater than its neighbors**.

* You may assume `nums[-1] = nums[n] = -∞`
* The array may contain multiple peaks — return the index of **any one** peak.
* You must solve it in **O(log n)** time complexity.

---

### 📌 Examples

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element.
```

**Example 2:**

```
Input: nums = [1,2,1,3,5,6,4]
Output: 1 or 5
Explanation: 2 and 6 are peak elements.
```

---

# 🔴 Brute Force Solution

### 💡 Idea

Check every element and verify if it is greater than its left and right neighbor.

---

### 🔹 Algorithm

1. Traverse from `i = 0` to `n-1`
2. For each element:

   * Check left neighbor (if exists)
   * Check right neighbor (if exists)
3. If it satisfies peak condition → return index

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size();
        
        for(int i = 0; i < n; i++) {
            bool left = (i == 0) || (nums[i] > nums[i - 1]);
            bool right = (i == n - 1) || (nums[i] > nums[i + 1]);
            
            if(left && right) return i;
        }
        
        return -1; // never reached
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

---

# 🟡 Better Solution (Improved Linear Thinking)

### 💡 Insight

If an element is greater than the next element, then a peak must exist on the **left side (including itself)**.

If an element is smaller than the next element, then a peak must exist on the **right side**.

This observation allows us to apply **Binary Search**.

---

# 🟢 Optimum Solution (Binary Search - O(log N))

### 💡 Core Idea

We use binary search based on slope:

* If `nums[mid] > nums[mid+1]` → peak lies on left side (including mid)
* Else → peak lies on right side

This works because:

* If the slope is increasing → peak must be right
* If slope is decreasing → peak must be left

---

### 🔹 Algorithm

1. Initialize `low = 0`, `high = n-1`
2. While `low < high`:

   * `mid = low + (high - low)/2`
   * If `nums[mid] > nums[mid+1]`
     → `high = mid`
   * Else
     → `low = mid + 1`
3. Return `low`

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int low = 0, high = nums.size() - 1;
        
        while(low < high) {
            int mid = low + (high - low) / 2;
            
            if(nums[mid] > nums[mid + 1]) {
                high = mid;
            } else {
                low = mid + 1;
            }
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

---

# 🎯 Why Binary Search Works

* There is always at least one peak.
* We are not searching for a specific value.
* We are searching based on **direction (slope property)**.
* Each step cuts the search space in half.

---

# 🏆 Final Comparison

| Approach                | Time     | Space |
| ----------------------- | -------- | ----- |
| Brute Force             | O(N)     | O(1)  |
| Binary Search (Optimal) | O(log N) | O(1)  |

---
