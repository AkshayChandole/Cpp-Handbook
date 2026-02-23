# [Binary Tree Zigzag Level Order Traversal](#binary-tree-zigzag-level-order-traversal)

## 📌 Problem Definition

Given the `root` of a binary tree, return the **zigzag level order traversal** of its nodes' values.

#### Zigzag Order:

* Level 1 → Left to Right
* Level 2 → Right to Left
* Level 3 → Left to Right
* and so on…

---

## 📌 Example

#### Example 1

```id="e6x1mh"
        3
       / \
      9   20
         /  \
        15   7
```

Output:

```id="pcjsy1"
[
  [3],
  [20, 9],
  [15, 7]
]
```

---

## ✅ Solutions

---

## 🔹 1. Brute Force Approach (Level Order + Reverse)

---

### 💡 Idea

1. Perform normal BFS level order.
2. Maintain a boolean flag `leftToRight`.
3. After each level:

   * If `false`, reverse the level vector.
4. Toggle flag.

---

### 💻 C++ Code (BFS + Reverse + Cleanup)

```cpp
#include <iostream>
#include <vector>
#include <queue>
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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> result;
        if (!root) return result;

        queue<TreeNode*> q;
        q.push(root);

        bool leftToRight = true;

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

            if (!leftToRight)
                reverse(level.begin(), level.end());

            result.push_back(level);
            leftToRight = !leftToRight;
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

    TreeNode* root = new TreeNode(3);
    root->left = new TreeNode(9);
    root->right = new TreeNode(20);
    root->right->left = new TreeNode(15);
    root->right->right = new TreeNode(7);

    Solution sol;
    vector<vector<int>> result = sol.zigzagLevelOrder(root);

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
* Reverse per level → still overall **O(n)**

### 🧠 Space Complexity

* Queue → **O(n)** worst case

---

## 🔹 2. Optimal Approach (Without Reverse – Index Placement)

---

### 💡 Idea

Instead of reversing:

* Create vector of size `levelSize`.
* If `leftToRight`:

  * Insert at index `i`
* Else:

  * Insert at index `levelSize - 1 - i`

This avoids reverse cost.

---

### 💻 C++ Code (Optimized + Cleanup)

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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> result;
        if (!root) return result;

        queue<TreeNode*> q;
        q.push(root);

        bool leftToRight = true;

        while (!q.empty()) {
            int size = q.size();
            vector<int> level(size);

            for (int i = 0; i < size; i++) {
                TreeNode* node = q.front();
                q.pop();

                int index = leftToRight ? i : size - 1 - i;
                level[index] = node->val;

                if (node->left)
                    q.push(node->left);
                if (node->right)
                    q.push(node->right);
            }

            leftToRight = !leftToRight;
            result.push_back(level);
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

    TreeNode* root = new TreeNode(3);
    root->left = new TreeNode(9);
    root->right = new TreeNode(20);
    root->right->left = new TreeNode(15);
    root->right->right = new TreeNode(7);

    Solution sol;
    vector<vector<int>> result = sol.zigzagLevelOrder(root);

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

* **O(n)**

### 🧠 Space Complexity

* Queue storage → **O(n)**

---

## 🔥 Final Comparison

| Approach           | Time | Space | Interview Preference |
| ------------------ | ---- | ----- | -------------------- |
| Reverse Each Level | O(n) | O(n)  | Acceptable           |
| Indexed Placement  | O(n) | O(n)  | Preferred            |

---





