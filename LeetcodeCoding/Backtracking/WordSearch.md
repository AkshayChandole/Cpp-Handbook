 # [Word Search](#Word-Search)

 ## 🟢 Word Search

---

### 📌 Problem

Given an `m x n` board of characters and a string `word`, return `true` if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where:

* Adjacent cells are **horizontally or vertically neighboring**
* The same cell may **not be used more than once**

---

### 📌 Example

```id="ex1"
Input:
board = [
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
word = "ABCCED"

Output: true
```

```id="ex2"
Input:
word = "ABCB"

Output: false
```

---

# 🔴 1. Brute Force (Try All Paths Without Pruning)

### 💡 Idea

* Start from every cell
* Try exploring all 4 directions
* No visited tracking (incorrect approach logically)

❌ This will reuse cells and give wrong results.

So brute force must track visited but without pruning early.

---

### 🔹 Time Complexity

Worst case:
[
O(M * N * 4^{L})
]

Where:

* `M*N` = number of cells
* `L` = length of word

Very expensive.

---

# 🟡 2. Better Solution (DFS + Visited Matrix)

### 💡 Idea

Use DFS from every starting cell.

Track visited cells using a separate matrix.

---

### 🔹 Algorithm

1. Loop through each cell
2. If board[i][j] == word[0]

   * Start DFS
3. In DFS:

   * Check bounds
   * Check character match
   * Mark visited
   * Explore 4 directions
   * Backtrack (unmark visited)

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    bool dfs(vector<vector<char>>& board, string& word,
             int i, int j, int index,
             vector<vector<bool>>& visited) {

        if(index == word.size())
            return true;

        int m = board.size();
        int n = board[0].size();

        if(i < 0 || j < 0 || i >= m || j >= n ||
           visited[i][j] ||
           board[i][j] != word[index])
            return false;

        visited[i][j] = true;

        bool found =
            dfs(board, word, i+1, j, index+1, visited) ||
            dfs(board, word, i-1, j, index+1, visited) ||
            dfs(board, word, i, j+1, index+1, visited) ||
            dfs(board, word, i, j-1, index+1, visited);

        visited[i][j] = false;  // backtrack
        return found;
    }

    bool exist(vector<vector<char>>& board, string word) {
        int m = board.size();
        int n = board[0].size();

        vector<vector<bool>> visited(m, vector<bool>(n, false));

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(board[i][j] == word[0]) {
                    if(dfs(board, word, i, j, 0, visited))
                        return true;
                }
            }
        }

        return false;
    }
};
```

---

### ⏱ Time Complexity

O(M * N * 4^L)

### 🗂 Space Complexity

O(M * N) for visited
O(L) recursion stack

---

# 🟢 3. Optimum Solution (DFS + In-place Marking)

### 💡 Optimization

Instead of using extra visited matrix:

* Temporarily modify board cell
* Restore it after recursion

This reduces extra space.

---

### 🔹 Key Trick

Mark cell as:

```cpp
board[i][j] = '#';
```

After DFS:

```cpp
board[i][j] = originalChar;
```

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    bool dfs(vector<vector<char>>& board, string& word,
             int i, int j, int index) {

        if(index == word.size())
            return true;

        int m = board.size();
        int n = board[0].size();

        if(i < 0 || j < 0 || i >= m || j >= n ||
           board[i][j] != word[index])
            return false;

        char temp = board[i][j];
        board[i][j] = '#';  // mark visited

        bool found =
            dfs(board, word, i+1, j, index+1) ||
            dfs(board, word, i-1, j, index+1) ||
            dfs(board, word, i, j+1, index+1) ||
            dfs(board, word, i, j-1, index+1);

        board[i][j] = temp;  // restore
        return found;
    }

    bool exist(vector<vector<char>>& board, string word) {
        int m = board.size();
        int n = board[0].size();

        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(board[i][j] == word[0]) {
                    if(dfs(board, word, i, j, 0))
                        return true;
                }
            }
        }

        return false;
    }
};
```

---

### ⏱ Time Complexity

O(M * N * 4^L)

### 🗂 Space Complexity

O(L) recursion stack
(No extra visited matrix)

---

# 🎯 Why Backtracking Is Required

* We must explore all possible paths
* Must undo choices (backtrack)
* Avoid reusing same cell
* DFS naturally models path exploration

---

# 🏆 Final Comparison

| Approach              | Time      | Extra Space | Recommended |
| --------------------- | --------- | ----------- | ----------- |
| Brute DFS             | O(MN·4^L) | Large       | ❌           |
| DFS + Visited         | O(MN·4^L) | O(MN)       | ✅           |
| In-place Backtracking | O(MN·4^L) | O(L)        | ⭐ Best      |

---
