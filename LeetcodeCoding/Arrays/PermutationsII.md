# [Permutations II](#permutations-ii)

## 🟢 Permutations II

---

### 📌 Problem

Given a collection of numbers `nums` that **might contain duplicates**, return **all possible unique permutations**.

---

### 📌 Example

```id="ex1"
Input: nums = [1,1,2]
Output:
[
 [1,1,2],
 [1,2,1],
 [2,1,1]
]
```

```id="ex2"
Input: nums = [1,2,3]
Output:
[
 [1,2,3],
 [1,3,2],
 [2,1,3],
 [2,3,1],
 [3,1,2],
 [3,2,1]
]
```

---

# 🔴 1. Brute Force (Generate All + Use Set)

---

### 💡 Idea

1. Generate all permutations normally.
2. Insert each permutation into a `set`.
3. Convert set to vector.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    void backtrack(vector<int>& nums,
                   vector<int>& path,
                   vector<bool>& used,
                   set<vector<int>>& result) {

        if(path.size() == nums.size()) {
            result.insert(path);
            return;
        }

        for(int i = 0; i < nums.size(); i++) {
            if(used[i]) continue;

            used[i] = true;
            path.push_back(nums[i]);

            backtrack(nums, path, used, result);

            path.pop_back();
            used[i] = false;
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        set<vector<int>> result;
        vector<int> path;
        vector<bool> used(nums.size(), false);

        backtrack(nums, path, used, result);

        return vector<vector<int>>(result.begin(), result.end());
    }
};
```

---

### ⏱ Time Complexity

O(n! log n!)

### 🗂 Space Complexity

O(n! )

❌ Inefficient due to set usage.

---

# 🟡 2. Better Solution (Sort + Skip Duplicates)

---

### 💡 Key Insight

To avoid duplicates:

1. Sort the array first.
2. Skip duplicates using:

```id="skip"
if(i > 0 && nums[i] == nums[i-1] && !used[i-1])
    continue;
```

---

### 🔹 Why This Works

* Only use duplicate number if previous identical number has been used.
* Prevents generating same branch twice.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    void backtrack(vector<int>& nums,
                   vector<int>& path,
                   vector<bool>& used,
                   vector<vector<int>>& result) {

        if(path.size() == nums.size()) {
            result.push_back(path);
            return;
        }

        for(int i = 0; i < nums.size(); i++) {

            if(used[i]) continue;

            if(i > 0 && nums[i] == nums[i-1] && !used[i-1])
                continue;

            used[i] = true;
            path.push_back(nums[i]);

            backtrack(nums, path, used, result);

            path.pop_back();
            used[i] = false;
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());

        vector<vector<int>> result;
        vector<int> path;
        vector<bool> used(nums.size(), false);

        backtrack(nums, path, used, result);

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(n!)

### 🗂 Space Complexity

O(n)

⭐ Standard optimal solution.

---

# 🟢 3. Optimum Solution (Swap-Based Backtracking + Set per Level)

---

### 💡 Idea

Instead of `used[]`, we:

* Fix one position at a time.
* Swap elements.
* Use a set at each recursion level to avoid duplicates.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    void backtrack(int index,
                   vector<int>& nums,
                   vector<vector<int>>& result) {

        if(index == nums.size()) {
            result.push_back(nums);
            return;
        }

        unordered_set<int> seen;

        for(int i = index; i < nums.size(); i++) {

            if(seen.count(nums[i]))
                continue;

            seen.insert(nums[i]);

            swap(nums[index], nums[i]);

            backtrack(index + 1, nums, result);

            swap(nums[index], nums[i]);
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> result;
        backtrack(0, nums, result);
        return result;
    }
};
```

---

### ⏱ Time Complexity

O(n!)

### 🗂 Space Complexity

O(n)

⭐ Cleaner swap-based approach.

---

# 🔍 Dry Run

Example:

```id="dry1"
[1,1,2]
```

Sorted:

```id="dry2"
[1,1,2]
```

Level 0:

* Pick first 1
* Skip second 1 (because previous unused duplicate)
* Pick 2

Avoid duplicate permutations.

---

# 🎯 Why Sorting Helps

Sorting groups duplicates together.

Then:

```id="logic"
If previous duplicate not used → skip
```

Prevents duplicate branches.

---

# 🏆 Final Comparison

| Approach             | Time         | Space | Recommended |
| -------------------- | ------------ | ----- | ----------- |
| Generate + Set       | O(n! log n!) | High  | ❌           |
| Sort + Skip          | O(n!)        | O(n)  | ⭐ Best      |
| Swap + Set per Level | O(n!)        | O(n)  | ⭐           |

---

# 🎯 Interview Insight

This problem tests:

* Backtracking
* Handling duplicates
* Pruning
* Recursion tree understanding

Related problems:

* Permutations I
* Subsets II
* Combination Sum II
* N-Queens

---
