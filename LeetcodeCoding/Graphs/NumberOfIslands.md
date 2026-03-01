# [Number of Islands](#Number-of-Islands)

## 🟢 Number of Islands

---

### 📌 Problem

Given an `m x n` 2D grid of `'1'`s (land) and `'0'`s (water), return the **number of islands**.

An island is formed by connecting adjacent lands **horizontally or vertically**.

You may assume all four edges of the grid are surrounded by water.

---

### 📌 Example

```id="ex1"
Input:
grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]

Output: 1
```

```id="ex2"
Input:
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]

Output: 3
```

---

# 🔴 1. Brute Force (DFS Without Marking Properly)

### 💡 Idea

For every cell:

* If it's `'1'`, explore neighbors
* But if we don’t mark visited → infinite loop

So marking visited is mandatory.

Brute force essentially becomes DFS/BFS traversal.

---

# 🟡 2. Better Solution (DFS + Visited Matrix)

### 💡 Idea

* Traverse each cell
* If cell is `'1'` and not visited:

  * Run DFS to mark entire island
  * Increment island count

---

### 🔹 Algorithm

1. Loop through grid
2. When land found:

   * DFS in 4 directions
   * Mark visited
3. Increase count

---

### 💻 Code (C++)

```cpp id="dfs1"
class Solution {
public:
    void dfs(vector<vector<char>>& grid,
             vector<vector<bool>>& visited,
             int i, int j) {

        int m = grid.size();
        int n = grid[0].size();

        if(i < 0 || j < 0 || i >= m || j >= n ||
           visited[i][j] || grid[i][j] == '0')
            return;

        visited[i][j] = true;

        dfs(grid, visited, i+1, j);
        dfs(grid, visited, i-1, j);
        dfs(grid, visited, i, j+1);
        dfs(grid, visited, i, j-1);
    }

    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        int n = grid[0].size();

        vector<vector<bool>> visited(m, vector<bool>(n, false));
        int count = 0;

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(grid[i][j] == '1' && !visited[i][j]) {
                    dfs(grid, visited, i, j);
                    count++;
                }
            }
        }

        return count;
    }
};
```

---

### ⏱ Time Complexity

O(M × N)

### 🗂 Space Complexity

O(M × N) (visited + recursion stack)

---

# 🟢 3. Optimum Solution (DFS with In-place Marking)

### 💡 Optimization

Instead of using `visited`, directly modify grid:

Change `'1'` → `'0'` once visited.

This saves extra space.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    void dfs(vector<vector<char>>& grid, int i, int j) {
        int m = grid.size();
        int n = grid[0].size();

        if(i < 0 || j < 0 || i >= m || j >= n ||
           grid[i][j] == '0')
            return;

        grid[i][j] = '0';  // mark visited

        dfs(grid, i+1, j);
        dfs(grid, i-1, j);
        dfs(grid, i, j+1);
        dfs(grid, i, j-1);
    }

    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        int count = 0;

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(grid[i][j] == '1') {
                    dfs(grid, i, j);
                    count++;
                }
            }
        }

        return count;
    }
};
```

---

### ⏱ Time Complexity

O(M × N)

### 🗂 Space Complexity

O(M × N) worst-case recursion stack
(No extra visited matrix)

⭐ Most commonly expected answer.

---

# 🔵 4. Alternative Optimal Approach (BFS)

### 💡 Idea

Instead of DFS:

* Use queue
* Level-order traversal to mark island

---

### 💻 Code (C++)

```cpp id="bfs1"
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        int count = 0;

        vector<pair<int,int>> directions = {
            {1,0}, {-1,0}, {0,1}, {0,-1}
        };

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(grid[i][j] == '1') {
                    count++;
                    queue<pair<int,int>> q;
                    q.push({i, j});
                    grid[i][j] = '0';

                    while(!q.empty()) {
                        auto [x, y] = q.front();
                        q.pop();

                        for(auto [dx, dy] : directions) {
                            int nx = x + dx;
                            int ny = y + dy;

                            if(nx >= 0 && ny >= 0 &&
                               nx < m && ny < n &&
                               grid[nx][ny] == '1') {

                                grid[nx][ny] = '0';
                                q.push({nx, ny});
                            }
                        }
                    }
                }
            }
        }

        return count;
    }
};
```

---

### ⏱ Time Complexity

O(M × N)

### 🗂 Space Complexity

O(min(M, N)) average
O(M × N) worst case (queue)

---

# 🎯 Interview Insight

* This is a classic **Connected Components in Grid**
* Pattern:

  * DFS / BFS
  * Flood fill
  * Mark visited
* Can be extended to:

  * Max Area of Island
  * Number of Closed Islands
  * Surrounded Regions
  * Rotting Oranges

---

# 🏆 Final Comparison

| Approach      | Time  | Extra Space  | Recommended |
| ------------- | ----- | ------------ | ----------- |
| DFS + Visited | O(MN) | O(MN)        | ✅           |
| DFS In-place  | O(MN) | O(recursion) | ⭐ Best      |
| BFS           | O(MN) | O(queue)     | ✅           |

---

