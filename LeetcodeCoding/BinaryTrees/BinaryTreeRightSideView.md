
# [Binary Tree Right Side View](#binary-tree-right-side-view)

## 📌 Problem Definition

Given the `root` of a binary tree, return the values of the nodes you can see when looking at the tree from the **right side**.

👉 At each level, only the **rightmost node** is visible.

---

## 📌 Example

#### Example 1

```
        1
       / \
      2   3
       \    \
        5    4
```

Output:

```
[1, 3, 4]
```

Explanation:

* Level 0 → 1
* Level 1 → 3
* Level 2 → 4

---

## ✅ Solutions

---

## 🔹 1. Brute Force (Level Order Traversal – BFS)

---

### 💡 Idea

Perform normal BFS.

At each level:

* Process all nodes
* Store the **last node's value**

That will be the visible node from right side.

---

### 💻 C++ Code (BFS + Memory Safe)

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
    vector<int> rightSideView(TreeNode* root) {
        vector<int> result;
        if (!root) return result;

        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            int size = q.size();

            for (int i = 0; i < size; i++) {
                TreeNode* node = q.front();
                q.pop();

                // Last node of the level
                if (i == size - 1)
                    result.push_back(node->val);

                if (node->left)
                    q.push(node->left);
                if (node->right)
                    q.push(node->right);
            }
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
            1
           / \
          2   3
           \    \
            5    4
    */

    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->right = new TreeNode(5);
    root->right->right = new TreeNode(4);

    Solution sol;
    vector<int> result = sol.rightSideView(root);

    for (int val : result)
        cout << val << " ";

    deleteTree(root);
    return 0;
}
```

---

### ⏱ Time Complexity

* Each node visited once → **O(n)**

### 🧠 Space Complexity

* Queue stores one level → **O(n)** worst case

---

## 🔹 2. Optimal DFS Approach (Right-First Traversal)

---

### 💡 Key Insight

If we traverse:

```
Root → Right → Left
```

The **first node we visit at each level** is the rightmost node.

So:

* Maintain current depth
* If depth == result.size(), add node

---

### 💻 C++ Code (DFS Right First + Memory Safe)

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
private:
    void dfs(TreeNode* root, int depth, vector<int>& result) {
        if (!root) return;

        // First node at this depth
        if (depth == result.size())
            result.push_back(root->val);

        // Go right first
        dfs(root->right, depth + 1, result);
        dfs(root->left, depth + 1, result);
    }

public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> result;
        dfs(root, 0, result);
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

    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->right = new TreeNode(5);
    root->right->right = new TreeNode(4);

    Solution sol;
    vector<int> result = sol.rightSideView(root);

    for (int val : result)
        cout << val << " ";

    deleteTree(root);
    return 0;
}
```

---

### ⏱ Time Complexity

* Each node visited once → **O(n)**

### 🧠 Space Complexity

* Recursive stack → **O(h)**

  * Worst case → **O(n)**
  * Balanced → **O(log n)**

---

## 🔥 Final Comparison

| Approach        | Time | Space | Interview Preference |
| --------------- | ---- | ----- | -------------------- |
| BFS Level Order | O(n) | O(n)  | Standard             |
| DFS Right-First | O(n) | O(h)  | More elegant         |

---





