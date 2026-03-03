# [Next Greater Element I](#next-greater-element-i)


## 🟢 Next Greater Element I

---

### 📌 Problem

You are given two arrays:

* `nums1` (subset of nums2)
* `nums2` (contains distinct elements)

For each element in `nums1`, find its **next greater element** in `nums2`.

The next greater element of `x` is:

* The first element to the right of `x` in `nums2` that is greater than `x`
* If none exists → return `-1`

---

### 📌 Example

```id="ex1"
Input:
nums1 = [4,1,2]
nums2 = [1,3,4,2]

Output:
[-1,3,-1]
```

Explanation:

```id="explain1"
4 → no greater element → -1
1 → next greater is 3
2 → no greater element → -1
```

---

```id="ex2"
Input:
nums1 = [2,4]
nums2 = [1,2,3,4]

Output:
[3,-1]
```

---

# 🔴 1. Brute Force

### 💡 Idea

For each element in `nums1`:

1. Find its index in `nums2`.
2. Traverse right side to find first greater element.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int findNextGreater(int num, vector<int>& nums2) {
        int n = nums2.size();
        int index = -1;

        for(int i = 0; i < n; i++) {
            if(nums2[i] == num) {
                index = i;
                break;
            }
        }

        for(int i = index + 1; i < n; i++) {
            if(nums2[i] > num)
                return nums2[i];
        }

        return -1;
    }

    vector<int> nextGreaterElement(vector<int>& nums1,
                                   vector<int>& nums2) {
        vector<int> result;

        for(int num : nums1)
            result.push_back(findNextGreater(num, nums2));

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N × M)

Where:

* N = size of nums1
* M = size of nums2

❌ Too slow for large inputs.

---

# 🟡 2. Better Solution (Precompute Using Stack)

### 💡 Key Insight

Instead of solving for each element separately:

* Precompute next greater for **all elements in nums2**
* Use **Monotonic Stack**

---

### 🔹 Monotonic Decreasing Stack

Traverse nums2:

* While stack not empty and current > stack.top()

  * Pop element
  * Current is next greater for popped element

Store results in hashmap.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1,
                                   vector<int>& nums2) {

        unordered_map<int, int> nextGreater;
        stack<int> st;

        for(int num : nums2) {
            while(!st.empty() && num > st.top()) {
                nextGreater[st.top()] = num;
                st.pop();
            }
            st.push(num);
        }

        while(!st.empty()) {
            nextGreater[st.top()] = -1;
            st.pop();
        }

        vector<int> result;

        for(int num : nums1)
            result.push_back(nextGreater[num]);

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N + M)

Each element pushed & popped once.

### 🗂 Space Complexity

O(M)

✅ Most expected solution.

---

# 🟢 3. Optimum Concept (Monotonic Stack Pattern)

### 💡 Why Monotonic Stack Works

We maintain:

```id="stack"
stack decreasing from bottom to top
```

So when a larger element appears:

* It resolves all smaller previous elements.

---

### 🔍 Dry Run

Example:

```id="dry1"
nums2 = [1,3,4,2]
```

Process:

| Current | Stack       | Action |
| ------- | ----------- | ------ |
| 1       | [1]         |        |
| 3       | pop 1 → 1→3 | [3]    |
| 4       | pop 3 → 3→4 | [4]    |
| 2       | [4,2]       |        |

Remaining:

```id="dry2"
4 → -1
2 → -1
```

Mapping:

```id="map"
1→3
3→4
4→-1
2→-1
```

---

# 🎯 Why This Is Optimal

* Each element handled once.
* No nested loops.
* Classic "Next Greater Element" template.

---

# 🏆 Final Comparison

| Approach        | Time   | Space | Recommended |
| --------------- | ------ | ----- | ----------- |
| Brute Force     | O(N×M) | O(1)  | ❌           |
| Monotonic Stack | O(N+M) | O(M)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* Monotonic stack
* Precomputation trick
* HashMap lookup optimization

Related problems:

* Next Greater Element II
* Daily Temperatures
* Largest Rectangle in Histogram
* Trapping Rain Water

---

