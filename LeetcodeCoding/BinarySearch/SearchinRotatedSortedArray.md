# [Search in Rotated Sorted Array](#search-in-rotated-sorted-array)

## 🟢 Search in Rotated Sorted Array

---

### 📌 Problem

You are given a **sorted array** that has been rotated at some pivot.

Find the index of a target value in **O(log n)** time.

If not found → return `-1`.

You may assume:

* All elements are unique.

---

### 📌 Example

```id="ex1"
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

```id="ex2"
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

```id="ex3"
Input: nums = [1], target = 0
Output: -1
```

---

# 🔴 1. Brute Force (Linear Search)

### 💡 Idea

Simply iterate through array and check each element.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int search(vector<int>& nums, int target) {
        for(int i = 0; i < nums.size(); i++) {
            if(nums[i] == target)
                return i;
        }
        return -1;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

❌ Not acceptable (problem requires O(log n)).

---

# 🟡 2. Better Solution (Find Pivot + Binary Search)

### 💡 Idea

1. First find pivot (smallest element index).
2. Determine which half target lies in.
3. Apply normal binary search in that half.

---

### 🔹 Step 1: Find Pivot

Modified binary search:

* If `nums[mid] > nums[high]`
  → pivot is right side
* Else
  → pivot is left side

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    int findPivot(vector<int>& nums) {
        int low = 0, high = nums.size() - 1;

        while(low < high) {
            int mid = low + (high - low) / 2;

            if(nums[mid] > nums[high])
                low = mid + 1;
            else
                high = mid;
        }

        return low;
    }

    int binarySearch(vector<int>& nums, int low, int high, int target) {
        while(low <= high) {
            int mid = low + (high - low) / 2;

            if(nums[mid] == target) return mid;
            else if(nums[mid] < target) low = mid + 1;
            else high = mid - 1;
        }
        return -1;
    }

    int search(vector<int>& nums, int target) {
        int n = nums.size();
        int pivot = findPivot(nums);

        if(target >= nums[pivot] && target <= nums[n-1])
            return binarySearch(nums, pivot, n-1, target);

        return binarySearch(nums, 0, pivot-1, target);
    }
};
```

---

### ⏱ Time Complexity

O(log N)

### 🗂 Space Complexity

O(1)

✅ Works well.

---

# 🟢 3. Optimum Solution (Single Binary Search)

### 💡 Key Insight

At any time:

* One half of the array is always sorted.

So during binary search:

* Determine which half is sorted.
* Check if target lies in sorted half.
* Narrow search space accordingly.

---

### 🔹 Algorithm

1. While `low <= high`
2. Find `mid`
3. If `nums[mid] == target` → return mid
4. If left half is sorted:

   * If target lies in left half → search left
   * Else → search right
5. Else right half is sorted:

   * If target lies in right half → search right
   * Else → search left

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int low = 0, high = nums.size() - 1;

        while(low <= high) {
            int mid = low + (high - low) / 2;

            if(nums[mid] == target)
                return mid;

            // Left half sorted
            if(nums[low] <= nums[mid]) {
                if(target >= nums[low] && target < nums[mid])
                    high = mid - 1;
                else
                    low = mid + 1;
            }
            // Right half sorted
            else {
                if(target > nums[mid] && target <= nums[high])
                    low = mid + 1;
                else
                    high = mid - 1;
            }
        }

        return -1;
    }
};
```

---

### ⏱ Time Complexity

O(log N)

### 🗂 Space Complexity

O(1)

⭐ Most expected interview solution.

---

# 🔍 Dry Run Example

Array:

```id="dry1"
[4,5,6,7,0,1,2], target = 0
```

Step 1:

```
mid = 7
Left half sorted (4-7)
Target not in left → go right
```

Step 2:

```
Search [0,1,2]
mid = 1
Right half sorted
Target in right → found
```

---

# 🎯 Why It Works

Even after rotation:

* At least one half remains sorted.
* Binary search logic still applies.
* We just need to identify sorted half.

---

# 🏆 Final Comparison

| Approach       | Time     | Space | Recommended |
| -------------- | -------- | ----- | ----------- |
| Linear Search  | O(N)     | O(1)  | ❌           |
| Pivot + Binary | O(log N) | O(1)  | ✅           |
| Single Binary  | O(log N) | O(1)  | ⭐ Best      |

---

