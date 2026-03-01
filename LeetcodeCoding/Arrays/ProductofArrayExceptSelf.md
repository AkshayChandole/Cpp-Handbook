# [Product of Array Except Self](#product-of-array-except-self)


## 🟢 Product of Array Except Self

---

### 📌 Problem

Given an integer array `nums`, return an array `answer` such that:

[
answer[i] = \prod_{j \ne i} nums[j]
]

Constraints:

* Do **not** use division.
* Time complexity must be **O(N)**.
* Output array does not count as extra space.

---

### 📌 Example

```id="ex1"
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

```id="ex2"
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

---

# 🔴 1. Brute Force (Nested Loops)

### 💡 Idea

For each index:

* Multiply all other elements.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> result(n, 1);

        for(int i = 0; i < n; i++) {
            for(int j = 0; j < n; j++) {
                if(i != j)
                    result[i] *= nums[j];
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

❌ Too slow.

---

# 🟡 2. Better Solution (Using Left & Right Arrays)

### 💡 Idea

For each index:

[
answer[i] = (product\ of\ elements\ left) \times (product\ of\ elements\ right)
]

Create two arrays:

* `left[i]` → product of elements before `i`
* `right[i]` → product of elements after `i`

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> left(n, 1), right(n, 1), result(n);

        for(int i = 1; i < n; i++)
            left[i] = left[i - 1] * nums[i - 1];

        for(int i = n - 2; i >= 0; i--)
            right[i] = right[i + 1] * nums[i + 1];

        for(int i = 0; i < n; i++)
            result[i] = left[i] * right[i];

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(N)

✅ Much better.

---

# 🟢 3. Optimum Solution (Space Optimized)

### 💡 Key Insight

We don’t need separate left and right arrays.

Use:

1. First pass → store left products in result.
2. Second pass → multiply with running right product.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> result(n, 1);

        // Left product
        for(int i = 1; i < n; i++)
            result[i] = result[i - 1] * nums[i - 1];

        // Right product
        int rightProduct = 1;
        for(int i = n - 1; i >= 0; i--) {
            result[i] *= rightProduct;
            rightProduct *= nums[i];
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1) (excluding output)

⭐ Most expected interview solution.

---

# 🔍 Dry Run Example

Input:

```id="dry1"
nums = [1,2,3,4]
```

### Step 1: Left products

```id="left"
result = [1,1,2,6]
```

### Step 2: Right products

```id="right"
Final result = [24,12,8,6]
```

---

# 🎯 Why No Division?

Division fails when:

* There are zeroes in the array.
* Problem explicitly restricts it.

Example:

```id="zero"
nums = [0,1,2]
```

Division approach breaks.

---

# 🏆 Final Comparison

| Approach            | Time  | Space | Recommended |
| ------------------- | ----- | ----- | ----------- |
| Brute Force         | O(N²) | O(1)  | ❌           |
| Left + Right Arrays | O(N)  | O(N)  | ✅           |
| Optimized Two Pass  | O(N)  | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Prefix & Suffix product pattern
* Space optimization
* Handling zero edge cases

Related problems:

* Trapping Rain Water (prefix/suffix)
* Maximum Product Subarray
* Prefix Sum problems

---
