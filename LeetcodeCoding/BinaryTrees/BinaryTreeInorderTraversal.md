# [Binary Tree Inorder Traversal](#binary-tree-inorder-traversal)

## 📌 Problem Definition

Given the root of a binary tree, return the **inorder traversal** of its nodes' values.

#### Inorder Traversal Order:

```
Left → Root → Right
```

---

## 📌 Examples

#### Example 1

Input:

```
    1
     \
      2
     /
    3
```

Output:

```
[1,3,2]
```

---

## ✅ Solutions

---

## 🔹 1. Brute Force (Recursive DFS)

---

### 💡 Approach

Inorder traversal using recursion:

1. Traverse left subtree
2. Visit root
3. Traverse right subtree

---

### 💻 C++ Code (Recursive + Proper Memory Cleanup)

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
    void inorder(TreeNode* root, vector<int>& result) {
        if (!root) return;

        inorder(root->left, result);
        result.push_back(root->val);
        inorder(root->right, result);
    }

    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        inorder(root, result);
        return result;
    }
};

// Proper deletion (Postorder delete)
void deleteTree(TreeNode* root) {
    if (!root) return;
    deleteTree(root->left);
    deleteTree(root->right);
    delete root;
}

int main() {
    /*
            1
             \
              2
             /
            3
    */

    TreeNode* root = new TreeNode(1);
    root->right = new TreeNode(2);
    root->right->left = new TreeNode(3);

    Solution sol;
    vector<int> result = sol.inorderTraversal(root);

    for (int val : result)
        cout << val << " ";

    deleteTree(root);   // ✅ Memory cleanup

    return 0;
}
```

---

### ⏱ Time Complexity

* Each node visited once → **O(n)**

### 🧠 Space Complexity

* Recursive stack height → **O(h)**

  * Worst case (skewed) → **O(n)**
  * Balanced → **O(log n)**

---

## 🔹 2. Better Approach (Iterative Using Stack)

---

### 💡 Approach

Simulate recursion using stack.

Algorithm:

1. Push all left nodes into stack
2. Pop node
3. Visit it
4. Move to right subtree

---

### 💻 C++ Code (Iterative + Cleanup)

```cpp
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        TreeNode* curr = root;

        while (curr || !st.empty()) {
            while (curr) {
                st.push(curr);
                curr = curr->left;
            }

            curr = st.top();
            st.pop();

            result.push_back(curr->val);

            curr = curr->right;
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
    TreeNode* root = new TreeNode(1);
    root->right = new TreeNode(2);
    root->right->left = new TreeNode(3);

    Solution sol;
    vector<int> result = sol.inorderTraversal(root);

    for (int val : result)
        cout << val << " ";

    deleteTree(root);   // ✅ Memory cleanup

    return 0;
}
```

---

### ⏱ Time Complexity

* **O(n)**

### 🧠 Space Complexity

* Stack size up to height → **O(h)**

---

## 🔹 3. Optimal Approach (Morris Traversal – O(1) Space)

---

### 💡 Approach

Morris Traversal avoids recursion and stack by temporarily modifying tree links.

Steps:

1. If left is NULL → visit node → move right
2. Else:

   * Find inorder predecessor
   * If predecessor->right is NULL:

     * Set predecessor->right = current
     * Move left
   * Else:

     * Restore pointer
     * Visit node
     * Move right

---

### 💻 C++ Code (Morris + Cleanup)

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
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        TreeNode* curr = root;

        while (curr) {
            if (!curr->left) {
                result.push_back(curr->val);
                curr = curr->right;
            } else {
                TreeNode* pred = curr->left;

                while (pred->right && pred->right != curr)
                    pred = pred->right;

                if (!pred->right) {
                    pred->right = curr;
                    curr = curr->left;
                } else {
                    pred->right = nullptr;
                    result.push_back(curr->val);
                    curr = curr->right;
                }
            }
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
    TreeNode* root = new TreeNode(1);
    root->right = new TreeNode(2);
    root->right->left = new TreeNode(3);

    Solution sol;
    vector<int> result = sol.inorderTraversal(root);

    for (int val : result)
        cout << val << " ";

    deleteTree(root);   // ✅ Memory cleanup

    return 0;
}
```

---

### ⏱ Time Complexity

* Each node processed at most twice → **O(n)**

### 🧠 Space Complexity

* No stack
* No recursion
* Only few pointers → **O(1)**

---

## 🔥 Final Comparison

| Approach  | Time | Space | Interview Depth |
| --------- | ---- | ----- | --------------- |
| Recursive | O(n) | O(h)  | Basic           |
| Iterative | O(n) | O(h)  | Standard        |
| Morris    | O(n) | O(1)  | Advanced        |

---
