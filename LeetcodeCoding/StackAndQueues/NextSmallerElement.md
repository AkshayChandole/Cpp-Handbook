# [Next Smaller Element](#Next-Smaller-Element)



## 📌 Problem Definition

Given an array `nums`, for each element find the **Next Smaller Element to the Right**.

If no such element exists → return `-1`.

---

## 📌 Example

#### Example 1

```text
Input:  [4, 8, 5, 2, 25]
Output: [2, 5, 2, -1, -1]
```

Explanation:

* 4 → next smaller is 2
* 8 → next smaller is 5
* 5 → next smaller is 2
* 2 → none → -1
* 25 → none → -1

---

## ❌ Brute Force Approach

For every element:

* Traverse right side
* Find first smaller element

```cpp
for i = 0 → n-1
    for j = i+1 → n-1
        if nums[j] < nums[i]
            answer
```

#### ⏱ Time Complexity

```
O(n²)
```

Not optimal.

---

## ✅ Optimal Approach (Monotonic Increasing Stack)

---

## 🔥 Core Idea

We use a **Monotonic Increasing Stack**.

Stack maintains elements in:

```
Increasing order (from bottom to top)
```

We traverse from **right to left**.

Why?

Because we want next smaller on the right.

---

## 🔹 Algorithm

1. Create empty stack
2. Traverse array from right to left:

   * While stack not empty AND stack.top() >= current:

     * pop stack
   * If stack empty → answer = -1
     Else → answer = stack.top()
   * Push current into stack

---

## 💻 C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

vector<int> nextSmallerElement(vector<int>& nums) {

    int n = nums.size();
    vector<int> result(n);
    stack<int> st;

    for (int i = n - 1; i >= 0; i--) {

        while (!st.empty() && st.top() >= nums[i]) {
            st.pop();
        }

        if (st.empty())
            result[i] = -1;
        else
            result[i] = st.top();

        st.push(nums[i]);
    }

    return result;
}

int main() {

    vector<int> nums = {4, 8, 5, 2, 25};

    vector<int> ans = nextSmallerElement(nums);

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

---

## 🧠 Space Complexity

```
O(n)
```

For stack and result array.

---

## 🔥 Why Traverse Right to Left?

Because:

We need:

```
Next element on the RIGHT
```

So we process right side first.

---

## 🔥 Dry Run Example

nums = [4, 8, 5, 2, 25]

Processing from right:

```
25 → stack empty → -1
2  → pop 25 → -1
5  → next smaller = 2
8  → next smaller = 5
4  → pop 8,5 → next smaller = 2
```

Final:

```
[2,5,2,-1,-1]
```


---

## 🎯 Final Summary

| Feature   | Value                      |
| --------- | -------------------------- |
| Pattern   | Monotonic Increasing Stack |
| Time      | O(n)                       |
| Space     | O(n)                       |
| Direction | Right → Left               |

---
