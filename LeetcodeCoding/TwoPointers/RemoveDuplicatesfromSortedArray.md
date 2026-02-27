# [Remove Duplicates from Sorted Array](#remove-duplicates-from-sorted-array)

## 🧩 Problem: Remove Duplicates from Sorted Array

Given a **sorted array** `nums`, remove the duplicates **in-place** such that each unique element appears only once.

Return the number of unique elements `k`.

* Modify the array in-place.
* Extra space must be **O(1)**.
* The first `k` elements should contain the unique elements.

---

## 🔎 Examples

**Example 1:**

```id="ex1"
Input: nums = [1,1,2]
Output: 2
Modified nums = [1,2,_]
```

**Example 2:**

```id="ex2"
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5
Modified nums = [0,1,2,3,4,_,_,_,_,_]
```

---

# 🐢 Brute Force Solution (Using Extra Array)

### 💡 Idea

Since array is sorted:

* Traverse and copy only unique elements into a new array.

---

### 🔹 Algorithm

1. Create a temporary vector.
2. Add first element.
3. For each element:

   * If different from last added → push into temp.
4. Copy temp back into original array.
5. Return temp size.

---

### ⏱ Time Complexity

* **O(n)**

### 📦 Space Complexity

* **O(n)** (extra array)

---

### 💻 C++ Code

```cpp id="bf1">
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.empty()) return 0;

        vector<int> temp;
        temp.push_back(nums[0]);

        for(int i = 1; i < nums.size(); i++) {
            if(nums[i] != nums[i - 1]) {
                temp.push_back(nums[i]);
            }
        }

        for(int i = 0; i < temp.size(); i++) {
            nums[i] = temp[i];
        }

        return temp.size();
    }
};
```

---

# 🚀 Better Solution (Two Pointer Technique)

### 💡 Key Idea

Since array is sorted:

* Duplicates are adjacent.
* Use slow & fast pointer.
* Place unique elements at correct positions.

---

### 🔹 Algorithm

1. If array empty → return 0.
2. Let `i = 0` (slow pointer).
3. For `j = 1` to `n-1`:

   * If `nums[j] != nums[i]`:

     * Increment `i`
     * Assign `nums[i] = nums[j]`
4. Return `i + 1`.

---

### ⏱ Time Complexity

* **O(n)**

### 📦 Space Complexity

* **O(1)**

---

### 💻 C++ Code (Recommended)

```cpp id="opt1">
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.empty()) return 0;

        int i = 0;

        for(int j = 1; j < nums.size(); j++) {
            if(nums[j] != nums[i]) {
                i++;
                nums[i] = nums[j];
            }
        }

        return i + 1;
    }
};
```

---

# ⚡ Why This Works

Because:

* Array is sorted.
* Unique elements appear only once in consecutive blocks.
* We overwrite duplicates as we go.
* No extra space needed.

---

# 🏆 Final Recommendation

| Approach    | Time       | Space    | Recommendation |
| ----------- | ---------- | -------- | -------------- |
| Extra Array | O(n)       | O(n)     | ❌ Not allowed  |
| Two Pointer | ⭐ **O(n)** | **O(1)** | ✅ Best         |

---
