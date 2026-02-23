# [Diameter of Binary Tree](#diameter-of-binary-tree)

## 📌 Problem Definition

Given the `root` of a binary tree, return the **diameter** of the tree.

#### Diameter Definition:

The diameter of a binary tree is the **length of the longest path between any two nodes** in the tree.

* The path **may or may not pass through the root**
* Length is measured in **number of edges**

---

## 📌 Example

#### Example 1

```
        1
       / \
      2   3
     / \
    4   5
```

Output:

```
3
```

Explanation:

Longest path: `4 → 2 → 1 → 3`
Edges count = **3**

---

## ✅ Solutions

---

## 🔹 1. Brute Force Approach (Top-Down)

---

### 💡 Idea

For every node:

1. Compute height of left subtree
2. Compute height of right subtree
3. Update diameter = max(diameter, leftHeight + rightHeight)
4. Recursively compute for left and right subtree

⚠️ Height gets recomputed multiple times → inefficient.

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

    int diameter(TreeNode* root) {
        if (!root) return 0;

        int leftHeight = height(root->left);
        int rightHeight = height(root->right);

        int currentDiameter = leftHeight + rightHeight;

        int leftDiameter = diameter(root->left);
        int rightDiameter = diameter(root->right);

        return max(currentDiameter, max(leftDiameter, rightDiameter));
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

    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);

    Solution sol;
    cout << sol.diameter(root) << endl;

    deleteTree(root);
    return 0;
}
```

---

### ⏱ Time Complexity

* Height calculation is O(n)
* Done for each node → **O(n²)** worst case

### 🧠 Space Complexity

* Recursive stack → **O(h)**

---

## 🔹 2. Optimal Approach (Bottom-Up DFS)

---

### 💡 Idea

Instead of computing height separately:

* Compute height while updating diameter.
* At each node:

  * Get left height
  * Get right height
  * Update diameter
  * Return height

#### Key Formula:

```
Diameter at node = leftHeight + rightHeight
Height = 1 + max(leftHeight, rightHeight)
```

We maintain a global variable for diameter.

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
private:
    int maxDiameter = 0;

    int height(TreeNode* root) {
        if (!root) return 0;

        int leftHeight = height(root->left);
        int rightHeight = height(root->right);

        // Update diameter
        maxDiameter = max(maxDiameter, leftHeight + rightHeight);

        // Return height
        return 1 + max(leftHeight, rightHeight);
    }

public:
    int diameter(TreeNode* root) {
        height(root);
        return maxDiameter;
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

    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);

    Solution sol;
    cout << sol.diameter(root) << endl;

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

