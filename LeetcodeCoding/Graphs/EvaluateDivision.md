# [Evaluate Division](#evaluate-division)

## 🟢 Evaluate Division

---

### 📌 Problem

You are given:

* `equations` → pairs of variables
* `values` → result of division
* `queries` → division queries

For each query `A / B`, return the result.

If no possible answer → return `-1.0`.

---

### 📌 Example

```id="ex1"
Input:
equations = [["a","b"],["b","c"]]
values = [2.0,3.0]
queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]

Output:
[6.0,0.5,-1.0,1.0,-1.0]
```

Explanation:

```
a / b = 2
b / c = 3

a / c = 2 * 3 = 6
b / a = 1 / 2 = 0.5
```

---

# 🔴 1. Brute Force (Try All Paths Recursively)

### 💡 Idea

For each query:

* Try every equation
* Multiply values
* Track visited variables

Very inefficient.

---

### ⏱ Time Complexity

Exponential in worst case

❌ Not practical.

---

# 🟡 2. Better Solution (Graph + DFS)

### 💡 Key Insight

Treat this as a **weighted directed graph**.

If:

```
a / b = 2
```

Then:

```
a → b weight = 2
b → a weight = 1/2
```

Now query becomes:

Find path from `A` to `B`, multiply weights.

---

## 🔹 Step 1: Build Graph

```cpp
unordered_map<string, vector<pair<string,double>>> graph;
```

---

## 🔹 Step 2: DFS to Find Product

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    unordered_map<string, vector<pair<string,double>>> graph;

    double dfs(string src, string dest, 
               unordered_set<string>& visited) {

        if(!graph.count(src))
            return -1.0;

        if(src == dest)
            return 1.0;

        visited.insert(src);

        for(auto& neighbor : graph[src]) {
            string next = neighbor.first;
            double value = neighbor.second;

            if(!visited.count(next)) {
                double result = dfs(next, dest, visited);
                if(result != -1.0)
                    return value * result;
            }
        }

        return -1.0;
    }

    vector<double> calcEquation(vector<vector<string>>& equations,
                                vector<double>& values,
                                vector<vector<string>>& queries) {

        // Build graph
        for(int i = 0; i < equations.size(); i++) {
            string A = equations[i][0];
            string B = equations[i][1];
            double val = values[i];

            graph[A].push_back({B, val});
            graph[B].push_back({A, 1.0 / val});
        }

        vector<double> result;

        for(auto& query : queries) {
            string A = query[0];
            string B = query[1];

            unordered_set<string> visited;
            result.push_back(dfs(A, B, visited));
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

Let:

* V = number of variables
* E = number of equations
* Q = number of queries

Worst case per query → O(V + E)

Total → O(Q * (V + E))

---

### 🗂 Space Complexity

O(V + E)

✅ Most common interview solution.

---

# 🟢 3. Optimum Solution (Union-Find with Weight)

### 💡 Even Better Idea

Use **Disjoint Set (Union-Find)** with weights.

Each node stores:

```
parent[x]
weight[x] = x / parent[x]
```

When unioning:

```
a / b = value
```

We connect roots and adjust weights accordingly.

---

## 🔹 Why Union-Find?

* Efficient for multiple queries.
* Avoid repeated DFS.
* Good when many queries exist.

---

### 💻 Code (C++ – Weighted Union-Find)

```cpp id="opt1"
class Solution {
public:
    unordered_map<string, string> parent;
    unordered_map<string, double> weight;

    string find(string x) {
        if(parent[x] != x) {
            string origParent = parent[x];
            parent[x] = find(parent[x]);
            weight[x] *= weight[origParent];
        }
        return parent[x];
    }

    void unite(string x, string y, double value) {
        if(!parent.count(x)) {
            parent[x] = x;
            weight[x] = 1.0;
        }
        if(!parent.count(y)) {
            parent[y] = y;
            weight[y] = 1.0;
        }

        string rootX = find(x);
        string rootY = find(y);

        if(rootX != rootY) {
            parent[rootX] = rootY;
            weight[rootX] = value * weight[y] / weight[x];
        }
    }

    vector<double> calcEquation(vector<vector<string>>& equations,
                                vector<double>& values,
                                vector<vector<string>>& queries) {

        for(int i = 0; i < equations.size(); i++) {
            unite(equations[i][0], equations[i][1], values[i]);
        }

        vector<double> result;

        for(auto& query : queries) {
            string A = query[0];
            string B = query[1];

            if(!parent.count(A) || !parent.count(B) ||
               find(A) != find(B)) {
                result.push_back(-1.0);
            } else {
                result.push_back(weight[A] / weight[B]);
            }
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

Almost O(1) per query (amortized)

Total ≈ O((V + Q) α(V))

(α = inverse Ackermann, nearly constant)

---

### 🗂 Space Complexity

O(V)

⭐ Most optimal for many queries.

---

# 🎯 Why Graph Works

Because equations form:

* Multiplicative relationships
* Transitive connections
* Weighted paths

So this becomes:

> Path multiplication problem in weighted graph.

---

# 🏆 Final Comparison

| Approach    | Time        | Space  | Recommended |
| ----------- | ----------- | ------ | ----------- |
| Brute Force | Exponential | High   | ❌           |
| Graph + DFS | O(Q(V+E))   | O(V+E) | ✅           |
| Union-Find  | Near O(Q)   | O(V)   | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Graph modeling
* DFS/BFS traversal
* Weighted Union-Find
* Mathematical relationship mapping

Related problems:

* Number of Connected Components
* Accounts Merge
* Redundant Connection
* Graph traversal with weights

---
