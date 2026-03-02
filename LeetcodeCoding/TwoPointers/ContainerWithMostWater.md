# [Container With Most Water](#container-with-most-water)


## 🟢 Container With Most Water

---

### 📌 Problem

You are given an integer array `height` where:

* `height[i]` represents the height of a vertical line at index `i`.

Find two lines that together with the x-axis form a container such that the container contains the **most water**.

Return the maximum amount of water.

---

### 📌 Formula

For two indices `i` and `j`:

```
Area = min(height[i], height[j]) * (j - i)
```

---

### 📌 Examples

```id="ex1"
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation:
Using height[1] = 8 and height[8] = 7
Area = min(8,7) * (8-1) = 7 * 7 = 49
```

```id="ex2"
Input: height = [1,1]
Output: 1
```

---

# 🔴 1. Brute Force (Check All Pairs)

### 💡 Idea

Try every pair `(i, j)`:

* Compute area
* Track maximum

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int maxArea(vector<int>& height) {
        int n = height.size();
        int maxWater = 0;

        for(int i = 0; i < n; i++) {
            for(int j = i + 1; j < n; j++) {
                int area = min(height[i], height[j]) * (j - i);
                maxWater = max(maxWater, area);
            }
        }

        return maxWater;
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(1)

❌ Too slow for large N.

---

# 🟡 2. Better Thinking (Observation)

### Key Observations

* Area depends on:

  * Width `(j - i)`
  * Smaller height `min(h[i], h[j])`
* Width decreases as pointers move inward.
* To improve area:

  * We must try increasing the smaller height.

---

# 🟢 3. Optimum Solution (Two Pointers)

### 💡 Core Idea

1. Start with:

   * `left = 0`
   * `right = n - 1`
2. Compute area.
3. Move the pointer with smaller height.
4. Repeat until pointers meet.

Why move smaller height?

Because:

* Area is limited by smaller height.
* Moving taller height cannot increase area if smaller remains same.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left = 0;
        int right = height.size() - 1;
        int maxWater = 0;

        while(left < right) {
            int h = min(height[left], height[right]);
            int width = right - left;
            maxWater = max(maxWater, h * width);

            if(height[left] < height[right])
                left++;
            else
                right--;
        }

        return maxWater;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

⭐ Most expected interview solution.

---

# 🔍 Dry Run

Example:

```id="dry1"
[1,8,6,2,5,4,8,3,7]
```

Initial:

```id="dry2"
left = 0 (1)
right = 8 (7)
Area = 1 * 8 = 8
```

Move left (since 1 < 7)

Next:

```id="dry3"
left = 1 (8)
right = 8 (7)
Area = 7 * 7 = 49
```

This is max.

---

# 🎯 Why Two Pointers Works

Suppose:

```id="logic"
height[left] < height[right]
```

Area = height[left] × width

If we move `right`:

* Width decreases
* Smaller height remains same
* Area cannot increase

So only hope is moving smaller pointer.

---

# 🏆 Final Comparison

| Approach     | Time  | Space | Recommended |
| ------------ | ----- | ----- | ----------- |
| Brute Force  | O(N²) | O(1)  | ❌           |
| Two Pointers | O(N)  | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* Two-pointer technique
* Greedy reasoning
* Why local decisions can lead to global optimum

Related problems:

* Trapping Rain Water
* 3Sum (two pointer)
* Maximum Area Histogram

---

