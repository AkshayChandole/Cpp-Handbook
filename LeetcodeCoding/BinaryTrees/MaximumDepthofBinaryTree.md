# [Maximum Depth of Binary Tree](#maximum-depth-of-binary-tree)

## 📌 Problem Definition

Given the `root` of a binary tree, return its **maximum depth**.

#### Definition:

The maximum depth is the number of nodes along the longest path
from the root node down to the farthest leaf node.

---

## 📌 Examples

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
3
```

---

#### Example 2

Input:

```
    1
     \
      2
```

Output:

```
2
```

---

## ✅ Solutions

---

## 🔹 1. Brute Force (Recursive DFS)

---

### 💡 Approach

Depth of tree =

```
1 + max(depth(left), depth(right))
```

Base case:

```
If root == NULL → return 0
```

This is the most natural and commonly expected solution.

---

### 💻 C++ Code (Recursive + Proper Memory Cleanup)

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (!root) return 0;

        int leftDepth = maxDepth(root->left);
        int rightDepth = maxDepth(root->right);

        return 1 + max(leftDepth, rightDepth);
    }
};

// Proper tree deletion (Postorder)
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
    cout << sol.maxDepth(root) << endl;

    deleteTree(root);  // ✅ Prevent memory leak
    return 0;
}
```

---

### ⏱ Time Complexity

* Each node visited once → **O(n)**

### 🧠 Space Complexity

* Recursive call stack → **O(h)**

  * Worst case (skewed tree) → **O(n)**
  * Balanced tree → **O(log n)**

---

## 🔹 2. Better / Iterative Approach (BFS – Level Order)

---

### 💡 Approach

Since depth equals number of levels:

1. Perform BFS.
2. Count number of levels.
3. Return level count.

---

### 💻 C++ Code (BFS + Cleanup)

```cpp
#include <iostream>
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
    int maxDepth(TreeNode* root) {
        if (!root) return 0;

        queue<TreeNode*> q;
        q.push(root);

        int depth = 0;

        while (!q.empty()) {
            int size = q.size();
            depth++;

            for (int i = 0; i < size; i++) {
                TreeNode* node = q.front();
                q.pop();

                if (node->left)
                    q.push(node->left);
                if (node->right)
                    q.push(node->right);
            }
        }

        return depth;
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
    cout << sol.maxDepth(root) << endl;

    deleteTree(root);  // ✅ Memory cleanup
    return 0;
}
```

---

### ⏱ Time Complexity

* Each node processed once → **O(n)**

### 🧠 Space Complexity

* Queue stores up to one full level
* Worst case (complete tree) → **O(n)**

---

## 🔥 Final Comparison

| Approach        | Time | Space | When to Use                    |
| --------------- | ---- | ----- | ------------------------------ |
| Recursive DFS   | O(n) | O(h)  | Most common                    |
| BFS Level Count | O(n) | O(n)  | When already doing level order |

---

