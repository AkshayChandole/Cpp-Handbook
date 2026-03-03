# [Largest Rectangle in Histogram](#largest-rectangle-in-histogram)

## 🟢 Largest Rectangle in Histogram

---

### 📌 Problem

Given an array `heights` representing histogram bar heights (width = 1), return the **area of the largest rectangle** in the histogram.

---

### 📌 Example

```id="ex1"
Input: heights = [2,1,5,6,2,3]
Output: 10
Explanation:
Largest rectangle is formed by heights 5 and 6
Area = 5 × 2 = 10
```

```id="ex2"
Input: heights = [2,4]
Output: 4
```

---

# 🔴 1. Brute Force (Check All Rectangles)

### 💡 Idea

For every index `i`:

* Expand left and right until smaller height found.
* Compute area.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        int maxArea = 0;

        for(int i = 0; i < n; i++) {
            int height = heights[i];
            int left = i, right = i;

            while(left >= 0 && heights[left] >= height)
                left--;

            while(right < n && heights[right] >= height)
                right++;

            int width = right - left - 1;
            maxArea = max(maxArea, height * width);
        }

        return maxArea;
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(1)

❌ Too slow.

---

# 🟡 2. Better Solution (Precompute Left & Right Boundaries)

### 💡 Idea

For each bar:

* Find first smaller element on left.
* Find first smaller element on right.

Use monotonic stack.

Then:

```id="formula"
area = height[i] × (right[i] - left[i] - 1)
```

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        vector<int> left(n), right(n);
        stack<int> st;

        // Left boundary
        for(int i = 0; i < n; i++) {
            while(!st.empty() && heights[st.top()] >= heights[i])
                st.pop();

            left[i] = st.empty() ? -1 : st.top();
            st.push(i);
        }

        while(!st.empty()) st.pop();

        // Right boundary
        for(int i = n - 1; i >= 0; i--) {
            while(!st.empty() && heights[st.top()] >= heights[i])
                st.pop();

            right[i] = st.empty() ? n : st.top();
            st.push(i);
        }

        int maxArea = 0;

        for(int i = 0; i < n; i++) {
            int width = right[i] - left[i] - 1;
            maxArea = max(maxArea, heights[i] * width);
        }

        return maxArea;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(N)

✅ Efficient.

---

# 🟢 3. Optimum Solution (Single Pass Monotonic Stack)

### 💡 Key Insight

Use a **monotonic increasing stack**.

When we find a smaller bar:

* It means current bar is right boundary
* Compute area for stack top

Add a dummy `0` height at end to flush stack.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> st;
        heights.push_back(0);  // Sentinel
        int maxArea = 0;

        for(int i = 0; i < heights.size(); i++) {
            while(!st.empty() && heights[st.top()] > heights[i]) {
                int height = heights[st.top()];
                st.pop();

                int width = st.empty() ? i : i - st.top() - 1;
                maxArea = max(maxArea, height * width);
            }
            st.push(i);
        }

        return maxArea;
    }
};
```

---

### ⏱ Time Complexity

O(N)

Each element pushed & popped once.

### 🗂 Space Complexity

O(N)

⭐ Most optimal and elegant solution.

---

# 🔍 Dry Run

Example:

```id="dry1"
[2,1,5,6,2,3]
```

When we hit height 2 after 6:

* Pop 6 → area = 6 × 1
* Pop 5 → area = 5 × 2
* Update max = 10

---

# 🎯 Why Monotonic Stack Works

The stack keeps indices of bars in increasing height.

When we see smaller height:

* It determines right boundary for all taller bars before it.

Width calculation:

```id="width"
if stack empty:
    width = i
else:
    width = i - stack.top() - 1
```

---

# 🏆 Final Comparison

| Approach              | Time  | Space | Recommended |
| --------------------- | ----- | ----- | ----------- |
| Expand Around         | O(N²) | O(1)  | ❌           |
| Precompute Boundaries | O(N)  | O(N)  | ✅           |
| Single Stack          | O(N)  | O(N)  | ⭐ Best      |

---

# 🎯 Interview Insight

This is a **very important monotonic stack problem**.

Pattern appears in:

* Maximal Rectangle (2D version)
* Daily Temperatures
* Next Greater Element
* Trapping Rain Water
* Sum of Subarray Minimums

---

