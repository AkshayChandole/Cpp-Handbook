# [Binary Tree Preorder Traversal](#binary-tree-preorder-traversal)


## 📌 Problem Definition

Given the root of a binary tree, return the **preorder traversal** of its nodes' values.

#### Preorder Traversal Order:

```
Root → Left → Right
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
[1,2,3]
```

---

## ✅ Solutions

---

## 🔹 1. Brute Force (Recursive DFS)

---

### 💡 Approach

Preorder traversal is naturally implemented using recursion:

1. Visit root
2. Traverse left subtree
3. Traverse right subtree

---

### 💻 C++ Code (Recursive with Proper Memory Cleanup)

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
    void preorder(TreeNode* root, vector<int>& result) {
        if (!root) return;

        result.push_back(root->val);
        preorder(root->left, result);
        preorder(root->right, result);
    }

    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        preorder(root, result);
        return result;
    }
};

// Proper tree deletion (Postorder deletion)
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
    vector<int> result = sol.preorderTraversal(root);

    for (int val : result)
        cout << val << " ";

    deleteTree(root);   // ✅ Prevent memory leak

    return 0;
}
```

---

### ⏱ Time Complexity

* Each node visited once → **O(n)**

### 🧠 Space Complexity

* Recursive stack → **O(h)**

  * Worst case: **O(n)**
  * Balanced tree: **O(log n)**

---

## 🔹 2. Iterative (Using Stack)

---

### 💡 Approach

Simulate recursion using stack:

1. Push root
2. Pop node
3. Push right child
4. Push left child

(Stack is LIFO, so left processed first)

---

### 💻 C++ Code (Iterative with Cleanup)

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
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        if (!root) return result;

        stack<TreeNode*> st;
        st.push(root);

        while (!st.empty()) {
            TreeNode* node = st.top();
            st.pop();

            result.push_back(node->val);

            if (node->right)
                st.push(node->right);
            if (node->left)
                st.push(node->left);
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
    vector<int> result = sol.preorderTraversal(root);

    for (int val : result)
        cout << val << " ";

    deleteTree(root);   // ✅ Prevent memory leak

    return 0;
}
```

---

### ⏱ Time Complexity

* **O(n)**

### 🧠 Space Complexity

* Stack stores up to height → **O(h)**

---

## 🔹 3. Optimal (Morris Traversal – O(1) Space)

---

### 💡 Approach

Uses threaded binary tree concept:

* No recursion
* No stack
* Temporary pointer rewiring
* Restores tree structure afterward

---

### 💻 C++ Code (Morris + Proper Cleanup)

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
    vector<int> preorderTraversal(TreeNode* root) {
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
                    result.push_back(curr->val);
                    pred->right = curr;
                    curr = curr->left;
                } else {
                    pred->right = nullptr;
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
    vector<int> result = sol.preorderTraversal(root);

    for (int val : result)
        cout << val << " ";

    deleteTree(root);   // ✅ Prevent memory leak

    return 0;
}
```

---

### ⏱ Time Complexity

* **O(n)**

### 🧠 Space Complexity

* **O(1)** extra space
* Only pointer manipulation

---

## 🔥 Final Comparison

| Approach  | Time | Space | Memory Safe |
| --------- | ---- | ----- | ----------- |
| Recursive | O(n) | O(h)  | ✅ Yes       |
| Iterative | O(n) | O(h)  | ✅ Yes       |
| Morris    | O(n) | O(1)  | ✅ Yes       |

---

#### 💡 Interview-Level Note

In real production C++ code, instead of raw pointers, you'd prefer:

```cpp
std::unique_ptr<TreeNode>
```

This automatically handles memory cleanup using RAII and eliminates manual delete calls.
