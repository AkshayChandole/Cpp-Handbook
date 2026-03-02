
# [Spiral Matrix](#Spiral-Matrix)

## 🟢 Spiral Matrix

---

### 📌 Problem

Given an `m x n` matrix, return **all elements of the matrix in spiral order**.

Spiral order means:

* Left → Right
* Top → Bottom
* Right → Left
* Bottom → Top
* Repeat…

---

### 📌 Example

```id="ex1"
Input:
matrix = [
 [1,2,3],
 [4,5,6],
 [7,8,9]
]

Output:
[1,2,3,6,9,8,7,4,5]
```

```id="ex2"
Input:
matrix = [
 [1,2,3,4],
 [5,6,7,8],
 [9,10,11,12]
]

Output:
[1,2,3,4,8,12,11,10,9,5,6,7]
```

---

# 🔴 1. Brute Force (Using Visited Matrix)

### 💡 Idea

* Maintain visited matrix.
* Move in directions: right, down, left, up.
* Change direction when hitting boundary or visited cell.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();

        vector<vector<bool>> visited(m, vector<bool>(n, false));
        vector<int> result;

        int dirs[4][2] = {{0,1},{1,0},{0,-1},{-1,0}};
        int dir = 0, row = 0, col = 0;

        for(int i = 0; i < m * n; i++) {
            result.push_back(matrix[row][col]);
            visited[row][col] = true;

            int newRow = row + dirs[dir][0];
            int newCol = col + dirs[dir][1];

            if(newRow < 0 || newCol < 0 ||
               newRow >= m || newCol >= n ||
               visited[newRow][newCol]) {
                dir = (dir + 1) % 4;
                newRow = row + dirs[dir][0];
                newCol = col + dirs[dir][1];
            }

            row = newRow;
            col = newCol;
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(M × N)

### 🗂 Space Complexity

O(M × N)

❌ Extra visited matrix not needed.

---

# 🟡 2. Better Solution (Boundary Simulation)

### 💡 Idea

Maintain 4 boundaries:

* `top`
* `bottom`
* `left`
* `right`

Traverse layer by layer.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> result;

        int top = 0;
        int bottom = matrix.size() - 1;
        int left = 0;
        int right = matrix[0].size() - 1;

        while(top <= bottom && left <= right) {

            // Left → Right
            for(int i = left; i <= right; i++)
                result.push_back(matrix[top][i]);
            top++;

            // Top → Bottom
            for(int i = top; i <= bottom; i++)
                result.push_back(matrix[i][right]);
            right--;

            if(top <= bottom) {
                // Right → Left
                for(int i = right; i >= left; i--)
                    result.push_back(matrix[bottom][i]);
                bottom--;
            }

            if(left <= right) {
                // Bottom → Top
                for(int i = bottom; i >= top; i--)
                    result.push_back(matrix[i][left]);
                left++;
            }
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(M × N)

### 🗂 Space Complexity

O(1) (excluding output)

⭐ Most expected interview solution.

---

# 🟢 3. Clean & Optimized Version

### 💡 Key Insight

We shrink boundaries after completing each direction.

Spiral is processed layer-by-layer.

---

### Why Boundary Checks Matter?

These conditions prevent double traversal in:

* Single row matrices
* Single column matrices
* Odd dimension matrices

```cpp
if(top <= bottom)
if(left <= right)
```

Without them → duplicates.

---

# 🔍 Dry Run

Matrix:

```id="dry1"
1 2 3
4 5 6
7 8 9
```

Steps:

```
Top row → 1 2 3
Right col → 6 9
Bottom row → 8 7
Left col → 4
Center → 5
```

Result:

```
1 2 3 6 9 8 7 4 5
```

---

# 🏆 Final Comparison

| Approach        | Time  | Space | Recommended |
| --------------- | ----- | ----- | ----------- |
| Visited Matrix  | O(MN) | O(MN) | ❌           |
| Boundary Method | O(MN) | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

Spiral problems teach:

* Boundary simulation
* Careful condition checks
* Layer-by-layer traversal

Related problems:

* Spiral Matrix II (generate spiral)
* Rotate Image
* Set Matrix Zeroes
* Zigzag traversal

---

