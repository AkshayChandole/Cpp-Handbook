# [Move Zeroes](#move-zeroes)

## 🟢 Move Zeroes

---

### 📌 Problem

Given an integer array `nums`, move all `0`s to the **end** of it while maintaining the **relative order of non-zero elements**.

You must do this **in-place** without making a copy of the array.

---

### 📌 Examples

```id="ex1"
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
```

```id="ex2"
Input: nums = [0]
Output: [0]
```

---

# 🔴 1. Brute Force (Using Extra Array)

### 💡 Idea

1. Create new array.
2. Insert all non-zero elements.
3. Fill remaining positions with zero.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        vector<int> temp;

        for(int num : nums) {
            if(num != 0)
                temp.push_back(num);
        }

        while(temp.size() < nums.size())
            temp.push_back(0);

        nums = temp;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(N)

❌ Not allowed (must be in-place).

---

# 🟡 2. Better Solution (Shift Non-Zero Elements)

### 💡 Idea

* Keep index `pos` for next non-zero placement.
* Traverse array:

  * If element ≠ 0 → place at `pos`.
  * Increment `pos`.
* After traversal → fill remaining with zeros.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int pos = 0;

        for(int i = 0; i < nums.size(); i++) {
            if(nums[i] != 0) {
                nums[pos] = nums[i];
                pos++;
            }
        }

        while(pos < nums.size()) {
            nums[pos] = 0;
            pos++;
        }
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

✅ Works well.

---

# 🟢 3. Optimum Solution (Two-Pointer Swap)

### 💡 Key Insight

Use two pointers:

* `left` → position for next non-zero
* `right` → current scanning pointer

When `nums[right] != 0`:

* Swap `nums[left]` and `nums[right]`
* Move `left++`

This avoids second pass for filling zeros.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int left = 0;

        for(int right = 0; right < nums.size(); right++) {
            if(nums[right] != 0) {
                swap(nums[left], nums[right]);
                left++;
            }
        }
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

⭐ Most optimal and clean.

---

# 🔍 Dry Run

Input:

```id="dry1"
[0,1,0,3,12]
```

Step-by-step:

| right | left | Action | Array        |
| ----- | ---- | ------ | ------------ |
| 0     | 0    | skip   | [0,1,0,3,12] |
| 1     | 0    | swap   | [1,0,0,3,12] |
| 2     | 1    | skip   | [1,0,0,3,12] |
| 3     | 1    | swap   | [1,3,0,0,12] |
| 4     | 2    | swap   | [1,3,12,0,0] |

Result = `[1,3,12,0,0]`

---

# 🎯 Why Two-Pointer Works

We:

* Maintain invariant:

  * All elements before `left` are non-zero.
  * All elements after processed area remain unchanged.
* Only swap when needed.
* Single pass.

---

# 🏆 Final Comparison

| Approach         | Time | Space | Recommended |
| ---------------- | ---- | ----- | ----------- |
| Extra Array      | O(N) | O(N)  | ❌           |
| Shift + Fill     | O(N) | O(1)  | ✅           |
| Two-Pointer Swap | O(N) | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* Two-pointer technique
* In-place modification
* Stable element rearrangement

Related problems:

* Remove Element
* Partition Array
* Sort Colors
* Move Negatives to End

---

