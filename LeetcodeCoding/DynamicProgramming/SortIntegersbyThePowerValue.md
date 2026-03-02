# [Sort Integers by The Power Value](#sort-integers-by-the-power-value)


## 🟢 Sort Integers by The Power Value

---

### 📌 Problem

The **power value** of an integer `x` is defined as:

* If `x` is even → `x = x / 2`
* If `x` is odd → `x = 3x + 1`
* Repeat until `x == 1`
* The number of steps required is its power value

Given:

* `lo`
* `hi`
* `k`

Return the **k-th integer** in range `[lo, hi]` sorted by:

1. Power value (ascending)
2. If tie → numerical value (ascending)

---

### 📌 Example

```id="ex1"
Input: lo = 12, hi = 15, k = 2
Output: 13
```

Power values:

```id="powers"
12 → 9 steps
13 → 9 steps
14 → 17 steps
15 → 17 steps
```

Sorted:

```id="sorted"
12, 13, 14, 15
```

2nd element → 13

---

# 🔴 1. Brute Force (Compute & Sort)

### 💡 Idea

1. Compute power value for each number in range.
2. Store in vector.
3. Sort by (power, number).
4. Return k-1 index.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int power(int x) {
        int steps = 0;
        while(x != 1) {
            if(x % 2 == 0)
                x /= 2;
            else
                x = 3 * x + 1;
            steps++;
        }
        return steps;
    }

    int getKth(int lo, int hi, int k) {
        vector<pair<int,int>> arr;

        for(int i = lo; i <= hi; i++) {
            arr.push_back({power(i), i});
        }

        sort(arr.begin(), arr.end());

        return arr[k - 1].second;
    }
};
```

---

### ⏱ Time Complexity

Let N = hi - lo

O(N × log(maxValue)) for power
O(N log N) for sorting

Overall ≈ O(N log N)

### 🗂 Space Complexity

O(N)

❌ Recomputes power repeatedly → inefficient.

---

# 🟡 2. Better Solution (Memoization)

### 💡 Key Insight

Power values repeat for many numbers.

Use memoization:

```id="memo"
unordered_map<int, int> dp
```

Store already computed power values.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    unordered_map<int,int> dp;

    int power(int x) {
        if(x == 1) return 0;
        if(dp.count(x)) return dp[x];

        if(x % 2 == 0)
            dp[x] = 1 + power(x / 2);
        else
            dp[x] = 1 + power(3 * x + 1);

        return dp[x];
    }

    int getKth(int lo, int hi, int k) {
        vector<pair<int,int>> arr;

        for(int i = lo; i <= hi; i++) {
            arr.push_back({power(i), i});
        }

        sort(arr.begin(), arr.end());

        return arr[k - 1].second;
    }
};
```

---

### ⏱ Time Complexity

Close to O(N log N)

But much faster in practice.

### 🗂 Space Complexity

O(N + recursion stack)

✅ Good optimization.

---

# 🟢 3. Optimum Solution (Use nth_element)

### 💡 Even Better

We don’t need full sort.

We only need k-th smallest element.

Use:

```cpp
nth_element()
```

This gives average O(N) selection.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    unordered_map<int,int> dp;

    int power(int x) {
        if(x == 1) return 0;
        if(dp.count(x)) return dp[x];

        if(x % 2 == 0)
            dp[x] = 1 + power(x / 2);
        else
            dp[x] = 1 + power(3 * x + 1);

        return dp[x];
    }

    int getKth(int lo, int hi, int k) {
        vector<pair<int,int>> arr;

        for(int i = lo; i <= hi; i++)
            arr.push_back({power(i), i});

        nth_element(arr.begin(), arr.begin() + k - 1, arr.end());

        sort(arr.begin(), arr.begin() + k);

        return arr[k - 1].second;
    }
};
```

---

### ⏱ Time Complexity

Average → O(N)

Worst → O(N²) (rare)

### 🗂 Space Complexity

O(N)

⭐ Most optimal practical solution.

---

# 🔍 Why Memoization Helps

Example:

```id="chain"
13 → 40 → 20 → 10 → 5 → 16 → 8 → 4 → 2 → 1
```

If later computing power(40):

We already know its value.

So we skip full recomputation.

---

# 🎯 Important Detail

Sorting rule:

```cpp id="rule"
sort by (power ascending)
if tie → number ascending
```

Using `pair<int,int>` automatically handles this.

---

# 🏆 Final Comparison

| Approach           | Time       | Space | Recommended |
| ------------------ | ---------- | ----- | ----------- |
| Brute Force        | O(N log N) | O(N)  | ❌           |
| Memoization + Sort | O(N log N) | O(N)  | ✅           |
| Memo + nth_element | Avg O(N)   | O(N)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Collatz sequence logic
* Memoization (DP on numbers)
* Custom sorting
* Selection algorithms

Related problems:

* Kth Largest Element
* Top K Frequent Elements
* Quick Select
* Dynamic Programming with caching

---
