# [All Paths From Source to Target](#all-paths-from-source-to-target)

## 🟢 All Paths From Source to Target

---

### 📌 Problem

Given a **Directed Acyclic Graph (DAG)** represented as:

```cpp
graph[i] = list of nodes you can visit from node i
```

Find all possible paths from:

```
source = 0
target = n - 1
```

Return all paths in any order.

---

### 📌 Example

```id="ex1"
Input: graph = [[1,2],[3],[3],[]]
Output: [[0,1,3],[0,2,3]]
```

Graph representation:

```
0 → 1 → 3
  → 2 → 3
```

---

# 🔴 1. Brute Force (Generate All Possible Paths)

### 💡 Idea

Try every possible path combination.

Since graph is DAG, no cycles.

But naive approach without pruning leads to exponential explosion.

---

### ⏱ Time Complexity

O(2^N) worst case (many paths)

❌ Not optimized.

---

# 🟡 2. Better Solution (DFS + Backtracking)

### 💡 Key Insight

This is classic:

> Find all paths in a DAG

Use DFS:

* Maintain current path.
* When reaching target → store path.
* Backtrack.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    void dfs(int node, vector<vector<int>>& graph,
             vector<int>& path,
             vector<vector<int>>& result) {

        path.push_back(node);

        if(node == graph.size() - 1) {
            result.push_back(path);
        } else {
            for(int neighbor : graph[node]) {
                dfs(neighbor, graph, path, result);
            }
        }

        path.pop_back();
    }

    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        vector<vector<int>> result;
        vector<int> path;

        dfs(0, graph, path, result);

        return result;
    }
};
```

---

### ⏱ Time Complexity

Let:

* V = nodes
* E = edges
* P = number of paths

Time ≈ O(P × V)

Because we copy path for each valid route.

### 🗂 Space Complexity

O(V) recursion stack
O(P × V) result storage

✅ Most expected solution.

---

# 🟢 3. Optimum Variation (Using Memoization)

### 💡 Even Better Idea

Since it’s DAG:

* Once we compute all paths from a node,
* We can cache it.

This avoids recomputation.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    vector<vector<int>> dfs(int node,
                            vector<vector<int>>& graph,
                            unordered_map<int, vector<vector<int>>>& memo) {

        if(memo.count(node))
            return memo[node];

        if(node == graph.size() - 1)
            return {{node}};

        vector<vector<int>> paths;

        for(int neighbor : graph[node]) {
            vector<vector<int>> nextPaths =
                dfs(neighbor, graph, memo);

            for(auto& path : nextPaths) {
                path.insert(path.begin(), node);
                paths.push_back(path);
            }
        }

        return memo[node] = paths;
    }

    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        unordered_map<int, vector<vector<int>>> memo;
        return dfs(0, graph, memo);
    }
};
```

---

### ⏱ Time Complexity

Still O(P × V)

But avoids recomputation in large DAGs.

### 🗂 Space Complexity

O(P × V)

⭐ Best for larger graphs.

---

# 🔍 Dry Run

Example:

```id="dry1"
graph = [[1,2],[3],[3],[]]
```

Call:

```id="dry2"
dfs(0)
```

Explores:

```
0 → 1 → 3
0 → 2 → 3
```

Store:

```id="dry3"
[[0,1,3], [0,2,3]]
```

---

# 🎯 Why No Visited Array Needed?

Because:

* Graph is DAG.
* No cycles guaranteed.
* So infinite recursion impossible.

---

# 🏆 Final Comparison

| Approach           | Time        | Space  | Recommended |
| ------------------ | ----------- | ------ | ----------- |
| Naive Enumeration  | Exponential | High   | ❌           |
| DFS + Backtracking | O(P×V)      | O(V)   | ✅           |
| DFS + Memo         | O(P×V)      | O(P×V) | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* DFS traversal
* Backtracking pattern
* Path building
* DAG property usage

Related problems:

* Path Sum II
* Word Search
* Combination Sum
* Course Schedule
* Topological Sort

---
