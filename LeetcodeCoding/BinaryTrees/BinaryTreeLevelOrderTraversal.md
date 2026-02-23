# [Binary Tree Level Order Traversal](#binary-tree-level-order-traversal)

## 📌 Problem Definition

Given the root of a binary tree, return the **level order traversal** of its nodes' values.

#### Level Order Traversal:

```
Traverse nodes level by level (Breadth First Search - BFS)
```

Return result as:

```
[
  [level1],
  [level2],
  [level3],
  ...
]
```

---

## 📌 Example

#### Example 1

Input:

```
        3
       / \
      9   20
         /  \
        15   7
```

Output:

```
[
  [3],
  [9,20],
  [15,7]
]
```

---

## ✅ Solutions

---

## 🔹 1. Brute Force (Recursive – Using Height)

---

### 💡 Approach

1. Calculate height of tree.
2. For each level `i` from `1 → height`
3. Print nodes at that level.

⚠️ This is inefficient because for each level we traverse the tree again.

---

### 💻 C++ Code (Recursive Height Based + Cleanup)

```cpp
#include <iostream>
#include <vector>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    int height(TreeNode* root) {
        if (!root) return 0;
        return 1 + max(height(root->left), height(root->right));
    }

    void printLevel(TreeNode* root, int level, vector<int>& currLevel) {
        if (!root) return;

        if (level == 1) {
            currLevel.push_back(root->val);
        } else {
            printLevel(root->left, level - 1, currLevel);
            printLevel(root->right, level - 1, currLevel);
        }
    }

    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        int h = height(root);

        for (int i = 1; i <= h; i++) {
            vector<int> currLevel;
            printLevel(root, i, currLevel);
            result.push_back(currLevel);
        }

        return result;
    }
};

void deleteTree(TreeNode* root) {
    if (!root) return;
    deleteTree(root->left);
    deleteTree(root->right);
    delete root;
}

int main() {
    TreeNode* root = new TreeNode(3);
    root->left = new TreeNode(9);
    root->right = new TreeNode(20);
    root->right->left = new TreeNode(15);
    root->right->right = new TreeNode(7);

    Solution sol;
    vector<vector<int>> result = sol.levelOrder(root);

    for (auto& level : result) {
        for (int val : level)
            cout << val << " ";
        cout << endl;
    }

    deleteTree(root);
    return 0;
}
```

---

### ⏱ Time Complexity

* Height calculation → O(n)
* For each level → O(n)
* Worst case (skewed tree height = n) → **O(n²)**

### 🧠 Space Complexity

* Recursive stack → O(h)

---

## 🔹 2. Optimal Approach (Queue – BFS)

---

### 💡 Approach

Use a queue (standard BFS):

1. Push root into queue.
2. While queue not empty:

   * Get current level size.
   * Process exactly that many nodes.
   * Push children into queue.
3. Store level result.

This ensures each node is visited once.

---

### 💻 C++ Code (BFS + Cleanup)

```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        if (!root) return result;

        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            int size = q.size();
            vector<int> level;

            for (int i = 0; i < size; i++) {
                TreeNode* node = q.front();
                q.pop();

                level.push_back(node->val);

                if (node->left)
                    q.push(node->left);
                if (node->right)
                    q.push(node->right);
            }

            result.push_back(level);
        }

        return result;
    }
};

void deleteTree(TreeNode* root) {
    if (!root) return;
    deleteTree(root->left);
    deleteTree(root->right);
    delete root;
}

int main() {
    TreeNode* root = new TreeNode(3);
    root->left = new TreeNode(9);
    root->right = new TreeNode(20);
    root->right->left = new TreeNode(15);
    root->right->right = new TreeNode(7);

    Solution sol;
    vector<vector<int>> result = sol.levelOrder(root);

    for (auto& level : result) {
        for (int val : level)
            cout << val << " ";
        cout << endl;
    }

    deleteTree(root);
    return 0;
}
```

---

### ⏱ Time Complexity

* Each node visited once → **O(n)**

### 🧠 Space Complexity

* Queue stores up to one level
* Worst case (complete tree) → **O(n)**

---

## 🔥 Final Comparison

| Approach     | Time        | Space | Interview Preference |
| ------------ | ----------- | ----- | -------------------- |
| Height Based | O(n²) worst | O(h)  | Not preferred        |
| BFS (Queue)  | O(n)        | O(n)  | Standard & Expected  |

---

## ⚡ Interview Insight

Level order traversal is:

* Pure BFS problem
* Foundation for:

  * Zigzag traversal
  * Right side view
  * Vertical order
  * Binary tree width
  * Serialize / Deserialize

---





