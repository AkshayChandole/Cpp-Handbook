# [Rotting Oranges](#rotting-oranges)

## 🟢 Rotting Oranges

---

### 📌 Problem

You are given an `m x n` grid where:

* `0` → empty cell
* `1` → fresh orange
* `2` → rotten orange

Every minute:

* Any fresh orange adjacent (4-directionally) to a rotten orange becomes rotten.

Return:

* Minimum number of minutes until no fresh orange remains.
* If impossible → return `-1`.

---

### 📌 Example

```id="ex1"
Input:
[
 [2,1,1],
 [1,1,0],
 [0,1,1]
]

Output: 4
```

```id="ex2"
Input:
[
 [2,1,1],
 [0,1,1],
 [1,0,1]
]

Output: -1
```

```id="ex3"
Input:
[[0,2]]
Output: 0
```

---

# 🔴 1. Brute Force (Simulate Minute by Minute)

### 💡 Idea

* For each minute:

  * Scan entire grid.
  * Convert adjacent fresh oranges.
* Repeat until no change.

---

### ❌ Why It’s Bad

Each minute scans full grid → O((m×n)²)

Too slow.

---

# 🟡 2. Better Solution (Multi-Source BFS)

---

### 💡 Key Insight

This is a classic **level-order BFS** problem.

Think of:

* All rotten oranges as initial sources.
* Each minute = one BFS level.

---

### 🔹 Steps

1. Push all rotten oranges into queue.
2. Count total fresh oranges.
3. Perform BFS:

   * For each level:

     * Spread rot to neighbors.
     * Decrease fresh count.
4. If fresh remains → return -1.
5. Else return minutes.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();

        queue<pair<int,int>> q;
        int fresh = 0;

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(grid[i][j] == 2)
                    q.push({i,j});
                else if(grid[i][j] == 1)
                    fresh++;
            }
        }

        if(fresh == 0)
            return 0;

        int minutes = 0;
        vector<int> dir = {0,1,0,-1,0};

        while(!q.empty()) {
            int size = q.size();
            bool rottenThisRound = false;

            for(int i = 0; i < size; i++) {
                auto [x, y] = q.front();
                q.pop();

                for(int d = 0; d < 4; d++) {
                    int nx = x + dir[d];
                    int ny = y + dir[d+1];

                    if(nx >= 0 && nx < m &&
                       ny >= 0 && ny < n &&
                       grid[nx][ny] == 1) {

                        grid[nx][ny] = 2;
                        fresh--;
                        q.push({nx, ny});
                        rottenThisRound = true;
                    }
                }
            }

            if(rottenThisRound)
                minutes++;
        }

        return fresh == 0 ? minutes : -1;
    }
};
```

---

### ⏱ Time Complexity

O(m × n)

Each cell processed once.

### 🗂 Space Complexity

O(m × n) (queue worst case)

⭐ Most optimal solution.

---

# 🔍 Why Multi-Source BFS?

Because:

* Multiple rotten oranges spread simultaneously.
* BFS naturally simulates time in levels.

---

# 🔎 Dry Run

Example:

```id="dry1"
2 1 1
1 1 0
0 1 1
```

Minute 0:

```id="dry2"
[2]
```

Minute 1:

```id="dry3"
2 2 1
2 1 0
0 1 1
```

Minute 2:

```id="dry4"
2 2 2
2 2 0
0 1 1
```

Minute 3:

```id="dry5"
2 2 2
2 2 0
0 2 1
```

Minute 4:

```id="dry6"
2 2 2
2 2 0
0 2 2
```

Answer = 4

---

# 🎯 Important Edge Cases

| Case              | Result          |
| ----------------- | --------------- |
| No fresh oranges  | 0               |
| Fresh unreachable | -1              |
| All empty         | 0               |
| Single cell       | handle properly |

---

# 🏆 Final Comparison

| Approach         | Time     | Space | Recommended |
| ---------------- | -------- | ----- | ----------- |
| Repeated Scan    | O((mn)²) | O(1)  | ❌           |
| Multi-Source BFS | O(mn)    | O(mn) | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* BFS
* Multi-source traversal
* Grid problems
* Level-order traversal logic

Related problems:

* Number of Islands
* Walls and Gates
* 01 Matrix
* Shortest Path in Binary Matrix

---

