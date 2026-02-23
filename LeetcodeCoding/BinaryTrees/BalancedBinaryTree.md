# [Balanced Binary Tree](#balanced-binary-tree)

## 📌 Problem Definition

Given the `root` of a binary tree, determine if it is **height-balanced**.

#### A binary tree is height-balanced if:

For every node,

```
| height(left subtree) - height(right subtree) | ≤ 1
```

---

## 📌 Examples

#### Example 1 (Balanced)

```
        3
       / \
      9   20
         /  \
        15   7
```

Output:

```
true
```

---

#### Example 2 (Not Balanced)

```
        1
       /
      2
     /
    3
```

Output:

```
false
```

---

## ✅ Solutions

---

## 🔹 1. Brute Force Approach

---

### 💡 Idea

For every node:

1. Compute height of left subtree
2. Compute height of right subtree
3. Check difference ≤ 1
4. Recursively check left and right subtree

⚠️ Problem: Height is recalculated repeatedly → inefficient

---

### 💻 C++ Code (Brute Force + Cleanup)

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
    int height(TreeNode* root) {
        if (!root) return 0;
        return 1 + max(height(root->left), height(root->right));
    }

    bool isBalanced(TreeNode* root) {
        if (!root) return true;

        int leftHeight = height(root->left);
        int rightHeight = height(root->right);

        if (abs(leftHeight - rightHeight) > 1)
            return false;

        return isBalanced(root->left) && isBalanced(root->right);
    }
};

// Proper deletion
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
    cout << (sol.isBalanced(root) ? "true" : "false") << endl;

    deleteTree(root);
    return 0;
}
```

---

### ⏱ Time Complexity

* For each node → height calculation takes O(n)
* Total → **O(n²)** worst case

### 🧠 Space Complexity

* Recursive stack → **O(h)**

---

## 🔹 2. Optimal Approach (Bottom-Up DFS)

---

### 💡 Idea

Instead of calculating height separately:

Return height while checking balance.

If subtree is unbalanced → return `-1` immediately.

#### Algorithm

```
height(node):
    if null → return 0
    left = height(left)
    if left == -1 → return -1
    right = height(right)
    if right == -1 → return -1
    if |left - right| > 1 → return -1
    return 1 + max(left, right)
```

Finally:

```
tree is balanced if height(root) != -1
```

This avoids repeated height calculations.

---

### 💻 C++ Code (Optimal + Cleanup)

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
    int checkHeight(TreeNode* root) {
        if (!root) return 0;

        int left = checkHeight(root->left);
        if (left == -1) return -1;

        int right = checkHeight(root->right);
        if (right == -1) return -1;

        if (abs(left - right) > 1)
            return -1;

        return 1 + max(left, right);
    }

    bool isBalanced(TreeNode* root) {
        return checkHeight(root) != -1;
    }
};

// Proper deletion
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
    cout << (sol.isBalanced(root) ? "true" : "false") << endl;

    deleteTree(root);
    return 0;
}
```

---

### ⏱ Time Complexity

* Each node visited once → **O(n)**

### 🧠 Space Complexity

* Recursive stack → **O(h)**

---

## 🔥 Final Comparison

| Approach      | Time  | Space | Interview Preference |
| ------------- | ----- | ----- | -------------------- |
| Brute Force   | O(n²) | O(h)  | ❌ Avoid              |
| Bottom-Up DFS | O(n)  | O(h)  | ✅ Expected           |

---






