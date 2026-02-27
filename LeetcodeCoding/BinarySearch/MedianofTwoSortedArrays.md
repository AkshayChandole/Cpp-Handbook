# [Median of Two Sorted Arrays](#median-of-two-sorted-arrays)

## 🧩 Problem: Median of Two Sorted Arrays

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return the **median** of the two sorted arrays.

The overall run time complexity should be **O(log (m+n))**.

---

## 🔎 Examples

**Example 1:**

```id="ex1"
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged = [1,2,3], median = 2
```

**Example 2:**

```id="ex2"
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged = [1,2,3,4], median = (2+3)/2
```

---

# 🐢 Brute Force Solution (Merge and Find Median)

### 💡 Idea

Merge both arrays into a single sorted array, then compute median.

---

### 🔹 Algorithm

1. Merge arrays using two pointers.
2. If total length is odd → return middle element.
3. If even → return average of two middle elements.

---

### ⏱ Time Complexity

* **O(m + n)**

### 📦 Space Complexity

* **O(m + n)**

---

### 💻 C++ Code

```cpp id="bf1">
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        vector<int> merged;
        int i = 0, j = 0;

        while(i < nums1.size() && j < nums2.size()) {
            if(nums1[i] < nums2[j])
                merged.push_back(nums1[i++]);
            else
                merged.push_back(nums2[j++]);
        }

        while(i < nums1.size()) merged.push_back(nums1[i++]);
        while(j < nums2.size()) merged.push_back(nums2[j++]);

        int n = merged.size();
        if(n % 2 == 1)
            return merged[n/2];
        else
            return (merged[n/2 - 1] + merged[n/2]) / 2.0;
    }
};
```

---

# 🚀 Better Solution (Without Extra Space)

### 💡 Idea

Instead of fully merging:

* Traverse like merge process.
* Stop when we reach median position.

---

### 🔹 Algorithm

1. Use two pointers.
2. Keep track of count until reaching `(m+n)/2`.
3. Maintain previous and current values.

---

### ⏱ Time Complexity

* **O(m + n)**

### 📦 Space Complexity

* **O(1)**

---

### 💻 C++ Code

```cpp id="better1">
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        int total = m + n;
        int mid1 = (total - 1) / 2;
        int mid2 = total / 2;

        int i = 0, j = 0, count = 0;
        int curr = 0, prev = 0;

        while(count <= mid2) {
            prev = curr;

            if(i < m && (j >= n || nums1[i] < nums2[j]))
                curr = nums1[i++];
            else
                curr = nums2[j++];

            count++;
        }

        if(total % 2 == 1)
            return curr;
        else
            return (prev + curr) / 2.0;
    }
};
```

---

# ⚡ Optimum Solution (Binary Search on Partition)

### 💡 Key Idea

We partition both arrays such that:

* Left half contains `(m+n+1)/2` elements
* Every element in left half ≤ every element in right half

We binary search on the **smaller array**.

---

### 🔹 Partition Logic

If partition at:

* `cut1` in nums1
* `cut2` in nums2

Then:

```
left1  = nums1[cut1-1]
right1 = nums1[cut1]

left2  = nums2[cut2-1]
right2 = nums2[cut2]
```

Valid partition if:

```
left1 <= right2  AND  left2 <= right1
```

---

### 🔹 Algorithm

1. Ensure `nums1` is smaller array.
2. Binary search on `nums1`.
3. Compute `cut1` and `cut2`.
4. Adjust search space based on partition validity.
5. Once valid:

   * If odd → max(left1, left2)
   * If even → average of max(left) and min(right)

---

### ⏱ Time Complexity

* **O(log(min(m, n)))**

### 📦 Space Complexity

* **O(1)**

---

### 💻 C++ Code (Recommended)

```cpp id="opt1">
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        if(nums1.size() > nums2.size())
            return findMedianSortedArrays(nums2, nums1);

        int m = nums1.size();
        int n = nums2.size();

        int low = 0, high = m;

        while(low <= high) {
            int cut1 = (low + high) / 2;
            int cut2 = (m + n + 1) / 2 - cut1;

            int left1  = (cut1 == 0) ? INT_MIN : nums1[cut1 - 1];
            int right1 = (cut1 == m) ? INT_MAX : nums1[cut1];

            int left2  = (cut2 == 0) ? INT_MIN : nums2[cut2 - 1];
            int right2 = (cut2 == n) ? INT_MAX : nums2[cut2];

            if(left1 <= right2 && left2 <= right1) {
                if((m + n) % 2 == 0)
                    return (max(left1, left2) + min(right1, right2)) / 2.0;
                else
                    return max(left1, left2);
            }
            else if(left1 > right2) {
                high = cut1 - 1;
            }
            else {
                low = cut1 + 1;
            }
        }

        return 0.0;
    }
};
```

---

# 🏆 Final Recommendation

| Approach                | Time                   | Space  | Recommendation |
| ----------------------- | ---------------------- | ------ | -------------- |
| Merge Array             | O(m+n)                 | O(m+n) | ❌ Not optimal  |
| Partial Merge           | O(m+n)                 | O(1)   | Good           |
| Binary Search Partition | ⭐ **O(log(min(m,n)))** | O(1)   | ✅ Best         |

👉 In interviews, always aim for the **binary search partition approach** — this is the expected optimal solution.


