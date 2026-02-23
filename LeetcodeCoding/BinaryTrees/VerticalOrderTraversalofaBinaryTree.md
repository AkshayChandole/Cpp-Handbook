
# [Vertical Order Traversal of a Binary Tree](#vertical-order-traversal-of-a-binary-tree)

## 📌 Problem Definition

Given the `root` of a binary tree, return its **vertical order traversal**.

For each node:

* Root → position `(row = 0, col = 0)`
* Left child → `(row + 1, col - 1)`
* Right child → `(row + 1, col + 1)`

#### Ordering Rules:

1. Sort by column (left → right)
2. Within same column → sort by row (top → bottom)
3. If same row & column → sort by node value

---

## 📌 Example

#### Example 1

```id="v7r6nq"
        3
       / \
      9   20
         /  \
        15   7
```

Output:

```id="o0s3mk"
[
  [9],
  [3,15],
  [20],
  [7]
]
```

---

## ✅ Solution (Optimal Approach)

---

## 🔹 Key Idea

We need to store:

```id="3k6zzx"
column → row → multiset(values)
```

Why multiset?

Because if two nodes have same row & column, we must sort values.

We use:

```id="s4zv8r"
map<int, map<int, multiset<int>>>
```

Structure:

```
column
   ↳ row
        ↳ sorted values
```

---

## 🔹 Algorithm

1. Use BFS with coordinates `(node, row, col)`
2. Insert into map structure
3. Traverse map column by column
4. Extract values in sorted order

---

## 💻 C++ Code (Complete + Memory Safe)

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <map>
#include <set>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        vector<vector<int>> result;
        if (!root) return result;

        // col -> row -> multiset(values)
        map<int, map<int, multiset<int>>> nodes;

        queue<pair<TreeNode*, pair<int,int>>> q;
        q.push({root, {0, 0}});  // node, {row, col}

        while (!q.empty()) {
            auto front = q.front();
            q.pop();

            TreeNode* node = front.first;
            int row = front.second.first;
            int col = front.second.second;

            nodes[col][row].insert(node->val);

            if (node->left)
                q.push({node->left, {row + 1, col - 1}});
            if (node->right)
                q.push({node->right, {row + 1, col + 1}});
        }

        // Extract result
        for (auto& colPair : nodes) {
            vector<int> column;

            for (auto& rowPair : colPair.second) {
                column.insert(column.end(),
                              rowPair.second.begin(),
                              rowPair.second.end());
            }

            result.push_back(column);
        }

        return result;
    }
};

// Proper cleanup
void deleteTree(TreeNode* root) {
    if (!root) return;
    deleteTree(root->left);
    deleteTree(root->right);
    delete root;
}

int main() {

    /*
            3
           / \
          9   20
             /  \
            15   7
    */

    TreeNode* root = new TreeNode(3);
    root->left = new TreeNode(9);
    root->right = new TreeNode(20);
    root->right->left = new TreeNode(15);
    root->right->right = new TreeNode(7);

    Solution sol;
    vector<vector<int>> result = sol.verticalTraversal(root);

    for (auto& col : result) {
        for (int val : col)
            cout << val << " ";
        cout << endl;
    }

    deleteTree(root);
    return 0;
}
```

---

## ⏱ Time Complexity

Let:

* n = number of nodes

#### Operations:

* Each insertion into map → O(log n)
* Each multiset insert → O(log n)

Total:

```id="0ptu9e"
O(n log n)
```

---

## 🧠 Space Complexity

* Map + multiset storage → O(n)
* Queue → O(n)
* Recursion stack (none)

Overall:

```id="ksndp0"
O(n)
```

---

## 🔥 Why This Is Hard

This problem tests:

* Coordinate-based tree traversal
* Ordered data structures
* Multi-level sorting logic
* Clean use of STL containers

---

## 🔥 Common Interview Mistake

Using:

```
map<int, vector<int>>
```

This fails because it does NOT maintain:

* Row ordering
* Value ordering when row & column match

---

## 🔥 Alternative Optimization

Instead of `map + multiset`, we can:

1. Store all nodes in vector:

   ```
   vector<tuple<col, row, value>>
   ```
2. Sort the vector
3. Group by column

Same complexity: O(n log n)

---
