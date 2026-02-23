
# [Boundary Traversal](#boundary-traversal)

## 📌 Problem Definition

Given the `root` of a binary tree, return the **boundary traversal** of the tree in anti-clockwise direction.

#### Boundary includes:

1️⃣ Root node
2️⃣ Left Boundary (excluding leaf nodes)
3️⃣ All Leaf Nodes (left to right)
4️⃣ Right Boundary (excluding leaf nodes, added in reverse)

---

## 📌 Example

```id="d0mfs6"
          1
        /   \
       2     3
      / \   / \
     4   5 6   7
        / \
       8   9
```

Output:

```id="0yq68m"
1 2 4 8 9 6 7 3
```

#### Explanation

* Root → `1`
* Left boundary → `2`
* Leaves → `4 8 9 6 7`
* Right boundary (bottom-up) → `3`

---

## ✅ Solution (Optimal – O(n))

---

## 🔹 Key Idea

We divide the problem into 3 helper functions:

```id="pp7s8e"
1. addLeftBoundary()
2. addLeaves()
3. addRightBoundary()
```

#### Important Rules

* Do NOT duplicate leaf nodes.
* Left boundary excludes leaves.
* Right boundary is added in reverse.

---

## 💻 C++ Code (Complete + Memory Safe)

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
    bool isLeaf(TreeNode* node) {
        return node && !node->left && !node->right;
    }

    void addLeftBoundary(TreeNode* root, vector<int>& result) {
        TreeNode* curr = root->left;
        while (curr) {
            if (!isLeaf(curr))
                result.push_back(curr->val);

            if (curr->left)
                curr = curr->left;
            else
                curr = curr->right;
        }
    }

    void addLeaves(TreeNode* root, vector<int>& result) {
        if (!root) return;

        if (isLeaf(root)) {
            result.push_back(root->val);
            return;
        }

        addLeaves(root->left, result);
        addLeaves(root->right, result);
    }

    void addRightBoundary(TreeNode* root, vector<int>& result) {
        TreeNode* curr = root->right;
        vector<int> temp;

        while (curr) {
            if (!isLeaf(curr))
                temp.push_back(curr->val);

            if (curr->right)
                curr = curr->right;
            else
                curr = curr->left;
        }

        // Add in reverse order
        for (int i = temp.size() - 1; i >= 0; i--)
            result.push_back(temp[i]);
    }

public:
    vector<int> boundaryTraversal(TreeNode* root) {
        vector<int> result;
        if (!root) return result;

        if (!isLeaf(root))
            result.push_back(root->val);

        addLeftBoundary(root, result);
        addLeaves(root, result);
        addRightBoundary(root, result);

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
            /   \
           2     3
          / \   / \
         4   5 6   7
            / \
           8   9
    */

    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);
    root->left->right->left = new TreeNode(8);
    root->left->right->right = new TreeNode(9);
    root->right->left = new TreeNode(6);
    root->right->right = new TreeNode(7);

    Solution sol;
    vector<int> result = sol.boundaryTraversal(root);

    for (int val : result)
        cout << val << " ";

    deleteTree(root);
    return 0;
}
```

---

## ⏱ Time Complexity

* Each node visited once → **O(n)**

---

## 🧠 Space Complexity

* Recursion stack for leaves → **O(h)**
* Extra vector for right boundary → **O(h)**

---


