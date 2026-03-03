# [Sort Colors](#Sort-Colors)

## 🟢 Sort Colors

---

### 📌 Problem

Given an array `nums` containing only:

```id="colors"
0 (red), 1 (white), 2 (blue)
```

Sort them **in-place** so that objects of the same color are adjacent in order:

```id="order"
0 → 1 → 2
```

You must not use library sort.

---

### 📌 Example

```id="ex1"
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

```id="ex2"
Input: nums = [2,0,1]
Output: [0,1,2]
```

---

# 🔴 1. Brute Force (Sort Using Built-in)

---

### 💡 Idea

Simply:

```cpp
sort(nums.begin(), nums.end());
```

---

### ⏱ Time Complexity

O(n log n)

### 🗂 Space Complexity

O(1)

❌ Not allowed per problem constraint.

---

# 🟡 2. Better Solution (Counting Sort – Two Pass)

---

### 💡 Idea

Since only 3 values:

1. Count number of 0s, 1s, 2s.
2. Overwrite array.

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int count0 = 0, count1 = 0, count2 = 0;

        for(int num : nums) {
            if(num == 0) count0++;
            else if(num == 1) count1++;
            else count2++;
        }

        int index = 0;

        while(count0--) nums[index++] = 0;
        while(count1--) nums[index++] = 1;
        while(count2--) nums[index++] = 2;
    }
};
```

---

### ⏱ Time Complexity

O(n)

### 🗂 Space Complexity

O(1)

✅ Efficient but 2 passes.

---

# 🟢 3. Optimum Solution (Dutch National Flag Algorithm – One Pass)

---

### 💡 Key Insight

Maintain three pointers:

```id="pointers"
low  → next position for 0
mid  → current element
high → next position for 2
```

---

### 🔹 Algorithm

While `mid <= high`:

* If `nums[mid] == 0`

  * Swap with `nums[low]`
  * `low++`, `mid++`
* If `nums[mid] == 1`

  * `mid++`
* If `nums[mid] == 2`

  * Swap with `nums[high]`
  * `high--` (don't increment mid)

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int low = 0;
        int mid = 0;
        int high = nums.size() - 1;

        while(mid <= high) {
            if(nums[mid] == 0) {
                swap(nums[low], nums[mid]);
                low++;
                mid++;
            }
            else if(nums[mid] == 1) {
                mid++;
            }
            else { // nums[mid] == 2
                swap(nums[mid], nums[high]);
                high--;
            }
        }
    }
};
```

---

### ⏱ Time Complexity

O(n)

Each element processed once.

### 🗂 Space Complexity

O(1)

⭐ Best solution.

---

# 🔍 Dry Run

Example:

```id="dry1"
[2,0,2,1,1,0]
```

Initial:

```id="dry2"
low=0, mid=0, high=5
```

Steps:

| Array         | Action          |
| ------------- | --------------- |
| [2,0,2,1,1,0] | swap mid & high |
| [0,0,2,1,1,2] | swap mid & low  |
| [0,0,2,1,1,2] | mid++           |
| [0,0,2,1,1,2] | swap mid & high |
| [0,0,1,1,2,2] | final           |

---

# 🎯 Why Don’t We Increment mid When Swapping with high?

Because:

* The swapped element from `high` hasn't been processed yet.
* It could be 0, 1, or 2.

So we re-check it.

---

# 🏆 Final Comparison

| Approach            | Time       | Space | Passes | Recommended |
| ------------------- | ---------- | ----- | ------ | ----------- |
| Built-in Sort       | O(n log n) | O(1)  | 1      | ❌           |
| Counting Sort       | O(n)       | O(1)  | 2      | ✅           |
| Dutch National Flag | O(n)       | O(1)  | 1      | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Two-pointer technique
* In-place sorting
* Partition logic
* Dutch National Flag pattern

Related problems:

* Partition Array
* Move Zeroes
* QuickSort partition step
* Wiggle Sort

---

