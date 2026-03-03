# [Merge Sorted Array](#merge-sorted-array)

## 🟢 Merge Sorted Array

---

### 📌 Problem

You are given two sorted integer arrays:

* `nums1` of size `m + n`

  * First `m` elements are valid
  * Last `n` elements are 0 (placeholders)
* `nums2` of size `n`

Merge `nums2` into `nums1` as one sorted array.

You must modify `nums1` **in-place**.

---

### 📌 Examples

```id="ex1"
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6], n = 3

Output:
[1,2,2,3,5,6]
```

```id="ex2"
Input:
nums1 = [1], m = 1
nums2 = [], n = 0

Output:
[1]
```

```id="ex3"
Input:
nums1 = [0], m = 0
nums2 = [1], n = 1

Output:
[1]
```

---

# 🔴 1. Brute Force (Append + Sort)

### 💡 Idea

1. Copy all elements of `nums2` into `nums1`.
2. Sort entire array.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    void merge(vector<int>& nums1, int m,
               vector<int>& nums2, int n) {

        for(int i = 0; i < n; i++)
            nums1[m + i] = nums2[i];

        sort(nums1.begin(), nums1.end());
    }
};
```

---

### ⏱ Time Complexity

O((m+n) log(m+n))

### 🗂 Space Complexity

O(1)

❌ Not optimal.

---

# 🟡 2. Better Solution (Merge Using Extra Array)

### 💡 Idea

Use classic merge process like merge sort.

* Compare elements from both arrays.
* Store in temporary array.
* Copy back to `nums1`.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    void merge(vector<int>& nums1, int m,
               vector<int>& nums2, int n) {

        vector<int> temp;
        int i = 0, j = 0;

        while(i < m && j < n) {
            if(nums1[i] <= nums2[j])
                temp.push_back(nums1[i++]);
            else
                temp.push_back(nums2[j++]);
        }

        while(i < m)
            temp.push_back(nums1[i++]);

        while(j < n)
            temp.push_back(nums2[j++]);

        for(int k = 0; k < m + n; k++)
            nums1[k] = temp[k];
    }
};
```

---

### ⏱ Time Complexity

O(m + n)

### 🗂 Space Complexity

O(m + n)

✅ Good but uses extra space.

---

# 🟢 3. Optimum Solution (Three Pointers from Back)

### 💡 Key Insight

Since `nums1` has extra space at end:

Merge from the **back**.

Why?

If we merge from front:

* We overwrite valid elements.

From back:

* We place largest elements first.
* No data loss.

---

### 🔹 Algorithm

Initialize:

```id="ptr"
i = m - 1
j = n - 1
k = m + n - 1
```

Compare:

* If nums1[i] > nums2[j]

  * nums1[k] = nums1[i]
  * i--
* Else

  * nums1[k] = nums2[j]
  * j--

Continue until `j < 0`.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    void merge(vector<int>& nums1, int m,
               vector<int>& nums2, int n) {

        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;

        while(i >= 0 && j >= 0) {
            if(nums1[i] > nums2[j])
                nums1[k--] = nums1[i--];
            else
                nums1[k--] = nums2[j--];
        }

        while(j >= 0)
            nums1[k--] = nums2[j--];
    }
};
```

---

### ⏱ Time Complexity

O(m + n)

### 🗂 Space Complexity

O(1)

⭐ Most expected interview solution.

---

# 🔍 Dry Run

Example:

```id="dry1"
nums1 = [1,2,3,0,0,0]
nums2 = [2,5,6]
```

Start:

```id="dry2"
i=2 (3)
j=2 (6)
k=5
```

Steps:

| Compare | Place | Array         |
| ------- | ----- | ------------- |
| 3 vs 6  | 6     | [1,2,3,0,0,6] |
| 3 vs 5  | 5     | [1,2,3,0,5,6] |
| 3 vs 2  | 3     | [1,2,3,3,5,6] |
| 2 vs 2  | 2     | [1,2,2,3,5,6] |
| 1 vs 2  | 2     | [1,2,2,3,5,6] |

Final:

```id="dry3"
[1,2,2,3,5,6]
```

---

# 🎯 Why Only Check j >= 0 at End?

Because:

If `nums2` finishes first:

* Remaining `nums1` elements already in correct position.

If `nums1` finishes first:

* We must copy remaining `nums2` elements.

---

# 🏆 Final Comparison

| Approach       | Time       | Space | Recommended |
| -------------- | ---------- | ----- | ----------- |
| Append + Sort  | O(N log N) | O(1)  | ❌           |
| Extra Array    | O(N)       | O(N)  | ❌           |
| Backward Merge | O(N)       | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Two-pointer technique
* In-place merging
* Reverse traversal trick

Related problems:

* Merge Two Sorted Lists
* Merge Intervals
* Sorted Squares
* Move Zeroes

---
