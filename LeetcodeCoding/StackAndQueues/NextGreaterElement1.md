# [Next Greater Element I](#Next-Greater-Element-I)


## 📌 Problem Definition

You are given two arrays:

* `nums1` (subset of nums2)
* `nums2`

For each element in `nums1`, find its **Next Greater Element** in `nums2`.

#### Next Greater Element:

The first greater element to the **right** of that element in `nums2`.

If none exists → return `-1`.

---

## 📌 Example

#### Example 1

```text
nums1 = [4,1,2]
nums2 = [1,3,4,2]
```

Output:

```text
[-1,3,-1]
```

Explanation:

* 4 → no greater on right → -1
* 1 → next greater is 3
* 2 → no greater → -1

---

#### Example 2

```text
nums1 = [2,4]
nums2 = [1,2,3,4]
```

Output:

```text
[3,-1]
```

---

## 🔥 Core Idea

This is a **Monotonic Stack** problem.

We want to preprocess `nums2` so that:

```text
For every element → store its next greater element
```

Then answer queries for `nums1` in O(1).

---

## ❌ Brute Force Approach

For each element in nums1:

* Find its position in nums2
* Traverse right until greater found

Time complexity:

```text
O(n * m)
```

Not optimal.

---

## ✅ Optimal Approach (Monotonic Decreasing Stack)

---

## 🔹 Key Observation

If we traverse nums2 from left to right:

Maintain stack such that:

```text
Stack always contains elements in decreasing order
```

When current element > stack.top():

* We found next greater for stack.top()
* Pop it
* Store mapping

---

## 🔹 Algorithm

1. Create empty stack
2. Create unordered_map<int,int> mp
3. Traverse nums2:

   * While stack not empty AND current > stack.top()

     * mp[stack.top()] = current
     * pop stack
   * push current
4. Remaining elements in stack → no next greater → map to -1
5. For nums1 → lookup in map

---

## 💻 C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <stack>
#include <unordered_map>
using namespace std;

class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1,
                                   vector<int>& nums2) {

        unordered_map<int, int> mp;
        stack<int> st;

        // Process nums2
        for (int num : nums2) {

            while (!st.empty() && num > st.top()) {
                mp[st.top()] = num;
                st.pop();
            }

            st.push(num);
        }

        // Remaining elements have no greater
        while (!st.empty()) {
            mp[st.top()] = -1;
            st.pop();
        }

        // Build result
        vector<int> result;
        for (int num : nums1) {
            result.push_back(mp[num]);
        }

        return result;
    }
};

int main() {

    vector<int> nums1 = {4,1,2};
    vector<int> nums2 = {1,3,4,2};

    Solution sol;
    vector<int> ans = sol.nextGreaterElement(nums1, nums2);

    for (int x : ans)
        cout << x << " ";

    return 0;
}
```

---

## ⏱ Time Complexity

Processing nums2:

```text
Each element pushed once
Each element popped once
→ O(n)
```

Processing nums1:

```text
O(m)
```

Total:

```text
O(n + m)
```

---

## 🧠 Space Complexity

```text
O(n)
```

For stack + hashmap.

---

## 🔥 Why Monotonic Stack Works

Example:

```text
nums2 = [1,3,4,2]
```

Stack process:

```
Push 1
3 > 1 → map[1]=3
Push 3
4 > 3 → map[3]=4
Push 4
2 < 4 → push 2
```

Remaining:

```
map[2] = -1
map[4] = -1
```

---


## 🎯 Final Summary

| Concept  | Value                                    |
| -------- | ---------------------------------------- |
| Pattern  | Monotonic Stack                          |
| Time     | O(n + m)                                 |
| Space    | O(n)                                     |
| Key Idea | Process nums2 once, answer nums1 in O(1) |

---
