
# [Flatten Binary Tree to Linked List](#Flatten-binary-tree-to-linked-list)


## 📌 Problem Definition

Given the `root` of a binary tree, flatten the tree into a **linked list in-place**.

#### Rules:

* The linked list should use the same `TreeNode` structure.
* The right child pointer points to the next node.
* The left child pointer should always be `NULL`.
* Order must follow **Preorder Traversal**:

```text
Root → Left → Right
```

---

## 📌 Example

#### Example

```id="9whd98"
        1
       / \
      2   5
     / \   \
    3   4   6
```

After flattening:

```id="5mgt3l"
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```

Output (as list):

```id="tsb17f"
[1,2,3,4,5,6]
```

---

## ✅ Solutions

---

## 🔹 1. Brute Force (Using Extra Vector)

---

### 💡 Idea

1. Perform preorder traversal.
2. Store nodes in vector.
3. Reconnect pointers sequentially.

⚠️ Uses extra space → Not optimal.

---

### 💻 C++ Code (Brute Force + Memory Safe)

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
    void preorder(TreeNode* root, vector<TreeNode*>& nodes) {
        if (!root) return;
        nodes.push_back(root);
        preorder(root->left, nodes);
        preorder(root->right, nodes);
    }

public:
    void flatten(TreeNode* root) {
        if (!root) return;

        vector<TreeNode*> nodes;
        preorder(root, nodes);

        for (int i = 0; i < nodes.size() - 1; i++) {
            nodes[i]->left = nullptr;
            nodes[i]->right = nodes[i + 1];
        }
    }
};

// Cleanup
void deleteTree(TreeNode* root) {
    if (!root) return;
    deleteTree(root->right); // since left is null after flatten
    delete root;
}

int main() {

    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(5);
    root->left->left = new TreeNode(3);
    root->left->right = new TreeNode(4);
    root->right->right = new TreeNode(6);

    Solution sol;
    sol.flatten(root);

    TreeNode* curr = root;
    while (curr) {
        cout << curr->val << " ";
        curr = curr->right;
    }

    deleteTree(root);
    return 0;
}
```

---

### ⏱ Time Complexity

* Preorder traversal → **O(n)**

### 🧠 Space Complexity

* Extra vector → **O(n)**

---

## 🔹 2. Better Approach (Recursive Reverse Preorder)

---

### 💡 Key Insight

Traverse in reverse preorder:

```text
Right → Left → Root
```

Maintain a pointer `prev`:

* Set `root->right = prev`
* Set `root->left = NULL`
* Update `prev = root`

---

### 💻 C++ Code (Optimal Recursive + Memory Safe)

```cpp
#include <iostream>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
private:
    TreeNode* prev = nullptr;

    void flattenTree(TreeNode* root) {
        if (!root) return;

        flattenTree(root->right);
        flattenTree(root->left);

        root->right = prev;
        root->left = nullptr;
        prev = root;
    }

public:
    void flatten(TreeNode* root) {
        flattenTree(root);
    }
};

// Cleanup
void deleteTree(TreeNode* root) {
    if (!root) return;
    deleteTree(root->right);
    delete root;
}

int main() {

    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(5);
    root->left->left = new TreeNode(3);
    root->left->right = new TreeNode(4);
    root->right->right = new TreeNode(6);

    Solution sol;
    sol.flatten(root);

    TreeNode* curr = root;
    while (curr) {
        cout << curr->val << " ";
        curr = curr->right;
    }

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

## 🔹 3. Optimal Approach (Iterative – Morris Style)

---

### 💡 Idea

For each node:

1. If left subtree exists:

   * Find rightmost node in left subtree.
   * Connect it to current's right subtree.
   * Move left subtree to right.
   * Set left to NULL.
2. Move to right.

This is similar to Morris traversal idea.

---

### 💻 C++ Code (O(1) Space + Memory Safe)

```cpp
#include <iostream>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    void flatten(TreeNode* root) {
        TreeNode* curr = root;

        while (curr) {
            if (curr->left) {
                TreeNode* prev = curr->left;

                while (prev->right)
                    prev = prev->right;

                prev->right = curr->right;
                curr->right = curr->left;
                curr->left = nullptr;
            }
            curr = curr->right;
        }
    }
};

// Cleanup
void deleteTree(TreeNode* root) {
    if (!root) return;
    deleteTree(root->right);
    delete root;
}

int main() {

    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(5);
    root->left->left = new TreeNode(3);
    root->left->right = new TreeNode(4);
    root->right->right = new TreeNode(6);

    Solution sol;
    sol.flatten(root);

    TreeNode* curr = root;
    while (curr) {
        cout << curr->val << " ";
        curr = curr->right;
    }

    deleteTree(root);
    return 0;
}
```

---

### ⏱ Time Complexity

* Each node processed once → **O(n)**

### 🧠 Space Complexity

* No recursion
* No extra data structure
  → **O(1)**

---

## 🔥 Final Comparison

| Approach          | Time | Space | Interview Preference |
| ----------------- | ---- | ----- | -------------------- |
| Vector (Brute)    | O(n) | O(n)  | ❌ Not ideal          |
| Recursive Reverse | O(n) | O(h)  | ✅ Good               |
| Morris Style      | O(n) | O(1)  | ⭐ Best               |

---
