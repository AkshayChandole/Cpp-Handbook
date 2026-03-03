# [Unique Paths II](#unique-paths-ii)

## 🟢 Unique Paths II

---

### 📌 Problem

You are given an `m x n` grid where:

* `0` → empty cell
* `1` → obstacle

A robot starts at **top-left (0,0)** and wants to reach **bottom-right (m-1,n-1)**.

The robot can move only:

* Right
* Down

Return the number of **unique paths** to reach the destination.

---

### 📌 Example

```id="ex1"
Input:
obstacleGrid =
[
 [0,0,0],
 [0,1,0],
 [0,0,0]
]

Output: 2
```

Explanation:

Only 2 valid paths avoiding obstacle.

---

```id="ex2"
Input:
[
 [0,1],
 [0,0]
]

Output: 1
```

---

# 🔴 1. Brute Force (Recursion)

### 💡 Idea

At each cell:

* Move right
* Move down
* If obstacle → return 0
* If reach destination → return 1

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int dfs(vector<vector<int>>& grid, int i, int j) {
        int m = grid.size();
        int n = grid[0].size();

        if(i >= m || j >= n || grid[i][j] == 1)
            return 0;

        if(i == m - 1 && j == n - 1)
            return 1;

        return dfs(grid, i + 1, j) +
               dfs(grid, i, j + 1);
    }

    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        return dfs(obstacleGrid, 0, 0);
    }
};
```

---

### ⏱ Time Complexity

O(2^(m+n)) → Exponential

### 🗂 Space Complexity

O(m+n) recursion stack

❌ Too slow.

---

# 🟡 2. Better Solution (Memoization – Top Down DP)

### 💡 Idea

Store results in DP table to avoid recomputation.

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int dfs(vector<vector<int>>& grid,
            int i, int j,
            vector<vector<int>>& dp) {

        int m = grid.size();
        int n = grid[0].size();

        if(i >= m || j >= n || grid[i][j] == 1)
            return 0;

        if(i == m - 1 && j == n - 1)
            return 1;

        if(dp[i][j] != -1)
            return dp[i][j];

        return dp[i][j] =
            dfs(grid, i + 1, j, dp) +
            dfs(grid, i, j + 1, dp);
    }

    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();

        vector<vector<int>> dp(m, vector<int>(n, -1));

        return dfs(obstacleGrid, 0, 0, dp);
    }
};
```

---

### ⏱ Time Complexity

O(m × n)

### 🗂 Space Complexity

O(m × n)

✅ Much better.

---

# 🟢 3. Optimum Solution (Bottom-Up DP)

---

### 💡 Key Insight

If cell is not obstacle:

```id="formula"
dp[i][j] = dp[i-1][j] + dp[i][j-1]
```

If obstacle:

```id="obs"
dp[i][j] = 0
```

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();

        vector<vector<long long>> dp(m, vector<long long>(n, 0));

        if(obstacleGrid[0][0] == 1)
            return 0;

        dp[0][0] = 1;

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(obstacleGrid[i][j] == 1) {
                    dp[i][j] = 0;
                } else {
                    if(i > 0)
                        dp[i][j] += dp[i - 1][j];
                    if(j > 0)
                        dp[i][j] += dp[i][j - 1];
                }
            }
        }

        return dp[m - 1][n - 1];
    }
};
```

---

### ⏱ Time Complexity

O(m × n)

### 🗂 Space Complexity

O(m × n)

⭐ Most standard solution.

---

# 🔥 4. Space Optimized Version (1D DP)

We only need previous row.

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();

        vector<long long> dp(n, 0);

        dp[0] = obstacleGrid[0][0] == 0 ? 1 : 0;

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(obstacleGrid[i][j] == 1)
                    dp[j] = 0;
                else if(j > 0)
                    dp[j] += dp[j - 1];
            }
        }

        return dp[n - 1];
    }
};
```

---

### ⏱ Time Complexity

O(m × n)

### 🗂 Space Complexity

O(n)

⭐ Most optimal.

---

# 🔍 Dry Run

Example:

```
0 0 0
0 1 0
0 0 0
```

DP table becomes:

```
1 1 1
1 0 1
1 1 2
```

Answer = 2

---

# 🏆 Final Comparison

| Approach    | Time        | Space  | Recommended |
| ----------- | ----------- | ------ | ----------- |
| Recursion   | Exponential | O(m+n) | ❌           |
| Memoization | O(mn)       | O(mn)  | ✅           |
| Bottom-Up   | O(mn)       | O(mn)  | ⭐           |
| 1D DP       | O(mn)       | O(n)   | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* 2D Dynamic Programming
* Grid traversal
* Handling obstacles
* Space optimization

Related problems:

* Unique Paths I
* Minimum Path Sum
* Number of Islands
* Climbing Stairs

---

