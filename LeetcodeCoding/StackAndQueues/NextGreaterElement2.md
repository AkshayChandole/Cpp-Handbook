# [Next Greater Element II](#Next-Greater-Element-II)


## 📌 Problem Definition

Given a **circular array** `nums`, return an array `result` such that:

```text
result[i] = Next Greater Element of nums[i]
```

If no greater element exists → return `-1`.

---

## 🔁 What Does Circular Mean?

After last element, it wraps back to first element.

Example:

```text
nums = [1,2,1]
```

For last `1`, we look at:

```text
2 (after wrapping)
```

---

## 📌 Example

#### Example 1

```text
Input:  [1,2,1]
Output: [2,-1,2]
```

Explanation:

* 1 → next greater is 2
* 2 → no greater → -1
* 1 → circular → next greater is 2

---

## ❌ Brute Force

For each element:

* Check next n elements (circular)

Time:

```
O(n²)
```

Not optimal.

---

## ✅ Optimal Approach (Monotonic Stack + 2 Pass Trick)

---

## 🔥 Core Idea

We simulate circular behavior by iterating:

```text
2 * n times
```

Using index:

```text
i % n
```

This ensures:

* Each element gets chance to see elements on right + beginning.

---

## 🔹 Algorithm

1. Create result array initialized with -1.
2. Create empty stack (store indices).
3. Traverse from 0 → 2*n - 1:

   * Let index = i % n
   * While stack not empty AND nums[index] > nums[stack.top()]

     * result[stack.top()] = nums[index]
     * pop stack
   * If i < n:

     * push index into stack

---

## 🔥 Why Push Only First n Elements?

Because:

We only want to compute answer once.

Second pass is only to resolve pending elements.

---

## 💻 C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {

        int n = nums.size();
        vector<int> result(n, -1);
        stack<int> st;  // store indices

        for (int i = 0; i < 2 * n; i++) {

            int index = i % n;

            while (!st.empty() &&
                   nums[index] > nums[st.top()]) {

                result[st.top()] = nums[index];
                st.pop();
            }

            if (i < n) {
                st.push(index);
            }
        }

        return result;
    }
};

int main() {

    vector<int> nums = {1,2,1};

    Solution sol;
    vector<int> ans = sol.nextGreaterElements(nums);

    for (int x : ans)
        cout << x << " ";

    return 0;
}
```

---

## ⏱ Time Complexity

Each element:

* Pushed once
* Popped once

```
O(n)
```

Even though loop runs 2n times.

---

## 🧠 Space Complexity

```
O(n)
```

For stack + result array.

---

## 🔥 Dry Run Example

nums = [1,2,1]

Iteration:

```
i=0 → 1 push
i=1 → 2 > 1 → result[0]=2
i=2 → 1 push
i=3 → (1 again) no change
i=4 → 2 > 1 → result[2]=2
```

Final:

```
[2,-1,2]
```

---

## 🔥 Why Monotonic Stack?

We maintain:

```
Stack contains decreasing elements
```

Whenever we see bigger element:

```
We resolve previous smaller elements
```

---

## 🎯 Final Summary

| Concept           | Value           |
| ----------------- | --------------- |
| Pattern           | Monotonic Stack |
| Circular Handling | Iterate 2*n     |
| Time              | O(n)            |
| Space             | O(n)            |

---
