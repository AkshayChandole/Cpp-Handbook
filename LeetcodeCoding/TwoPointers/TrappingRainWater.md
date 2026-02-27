# [Trapping Rain Water](#trapping-rain-water)

## 🧩 Problem: Trapping Rain Water

Given an array `height[]` where `height[i]` represents the elevation at index `i`, compute how much **water it can trap after raining**.

---

## 🔎 Examples

### Example 1

```text id="ex1"
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```

### Example 2

```text id="ex2"
Input: height = [4,2,0,3,2,5]
Output: 9
```

---

# 🔍 Core Formula

At index `i`, water trapped:

[
water[i] = \min(maxLeft[i], maxRight[i]) - height[i]
]

Where:

* `maxLeft[i]` = maximum height to the left of `i`
* `maxRight[i]` = maximum height to the right of `i`

---

# 🐢 Brute Force Solution

### 💡 Idea

For each index:

* Compute left max
* Compute right max
* Add trapped water

---

### ⏱ Time Complexity

* For each index → O(n)
* Total → **O(n²)**

### 📦 Space Complexity

* **O(1)**

---

### 💻 C++ Code

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        int total = 0;

        for(int i = 0; i < n; i++) {
            int leftMax = 0, rightMax = 0;

            for(int j = 0; j <= i; j++)
                leftMax = max(leftMax, height[j]);

            for(int j = i; j < n; j++)
                rightMax = max(rightMax, height[j]);

            total += min(leftMax, rightMax) - height[i];
        }

        return total;
    }
};
```

---

# 🚀 Better Solution (Prefix & Suffix Arrays)

### 💡 Idea

Precompute:

* `leftMax[]`
* `rightMax[]`

Then compute answer in one pass.

---

### ⏱ Time Complexity

* **O(n)**

### 📦 Space Complexity

* **O(n)**

---

### 💻 C++ Code

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        if(n == 0) return 0;

        vector<int> leftMax(n), rightMax(n);

        leftMax[0] = height[0];
        for(int i = 1; i < n; i++)
            leftMax[i] = max(leftMax[i-1], height[i]);

        rightMax[n-1] = height[n-1];
        for(int i = n-2; i >= 0; i--)
            rightMax[i] = max(rightMax[i+1], height[i]);

        int total = 0;
        for(int i = 0; i < n; i++)
            total += min(leftMax[i], rightMax[i]) - height[i];

        return total;
    }
};
```

---

# ⚡ Optimum Solution (Two Pointer Technique)

### 💡 Key Insight

Water level is determined by the **smaller boundary**.

Use two pointers:

* `left = 0`
* `right = n-1`

Maintain:

* `leftMax`
* `rightMax`

Move pointer with smaller height.

---

### 🔹 Algorithm

1. While `left < right`:

   * If `height[left] < height[right]`:

     * If `height[left] >= leftMax`

       * update leftMax
     * else

       * add `leftMax - height[left]`
     * `left++`
   * Else:

     * If `height[right] >= rightMax`

       * update rightMax
     * else

       * add `rightMax - height[right]`
     * `right--`

---

### ⏱ Time Complexity

* **O(n)**

### 📦 Space Complexity

* **O(1)**

---

### 💻 C++ Code (Recommended)

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        if(n == 0) return 0;

        int left = 0, right = n - 1;
        int leftMax = 0, rightMax = 0;
        int total = 0;

        while(left < right) {
            if(height[left] < height[right]) {
                if(height[left] >= leftMax)
                    leftMax = height[left];
                else
                    total += leftMax - height[left];
                left++;
            }
            else {
                if(height[right] >= rightMax)
                    rightMax = height[right];
                else
                    total += rightMax - height[right];
                right--;
            }
        }

        return total;
    }
};
```

---

# 🏆 Final Recommendation

| Approach      | Time       | Space    | Recommendation |
| ------------- | ---------- | -------- | -------------- |
| Brute Force   | O(n²)      | O(1)     | ❌ Too slow     |
| Prefix Arrays | O(n)       | O(n)     | Good           |
| Two Pointer   | ⭐ **O(n)** | **O(1)** | ✅ Best         |

---

