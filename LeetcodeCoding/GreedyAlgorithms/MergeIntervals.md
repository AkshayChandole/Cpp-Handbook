# [Merge Intervals](#merge-intervals)



## 🧩 Merge Intervals

Given an array of intervals `intervals` where
`intervals[i] = [start_i, end_i]`, merge all overlapping intervals and return the non-overlapping intervals covering all intervals in the input.

---

## 🔎 Examples

### Example 1

```text
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
```

### Example 2

```text
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
```

---

# 🐢 Brute Force Solution

### 💡 Idea

Compare every interval with every other interval and merge overlapping ones repeatedly until no changes remain.

This involves:

* Nested comparisons
* Rebuilding interval list multiple times

---

### ⏱ Time Complexity

* Pairwise comparisons → **O(n²)**

### 📦 Space Complexity

* **O(n)**

❌ Inefficient for large inputs.

---

# 🚀 Better / Optimal Solution (Greedy + Sorting)

### 💡 Key Insight

After sorting by start time:

* Overlapping intervals will be adjacent.
* We only need to compare with the last merged interval.

This is a **Greedy approach**.

---

# 🔹 Algorithm

1. If intervals empty → return empty.
2. Sort intervals by starting time.
3. Initialize result with first interval.
4. For each interval:

   * If current.start ≤ last.end:

     * Merge → `last.end = max(last.end, current.end)`
   * Else:

     * Push current interval
5. Return result.

---

# 🔍 Overlap Condition

Two intervals overlap if:

[
current.start \le last.end
]

---

# ⏱ Time Complexity

* Sorting → **O(n log n)**
* One pass merge → **O(n)**
* Total → ⭐ **O(n log n)**

---

# 📦 Space Complexity

* **O(n)** (for result)
* Sorting in-place (no extra sorting space required)

---

# 💻 C++ Code (Recommended)

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        if(intervals.empty())
            return {};

        sort(intervals.begin(), intervals.end());

        vector<vector<int>> result;
        result.push_back(intervals[0]);

        for(int i = 1; i < intervals.size(); i++) {
            if(intervals[i][0] <= result.back()[1]) {
                result.back()[1] = max(result.back()[1], intervals[i][1]);
            } else {
                result.push_back(intervals[i]);
            }
        }

        return result;
    }
};
```

---

# 🏆 Final Recommendation

| Approach            | Time             | Space | Recommendation |
| ------------------- | ---------------- | ----- | -------------- |
| Brute Force         | O(n²)            | O(n)  | ❌ Too slow     |
| Sort + Greedy Merge | ⭐ **O(n log n)** | O(n)  | ✅ Best         |

---

# 🧠 Why This Is Greedy

At each step, we:

* Merge overlapping intervals immediately (local optimal choice).
* Never need to revisit earlier decisions.
* Sorting ensures correctness.

---
