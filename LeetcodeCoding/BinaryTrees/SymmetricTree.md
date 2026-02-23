# [Symmetric Tree](#symmetric-tree)

## 📌 Problem Definition

Given the `root` of a binary tree, check whether it is a **mirror of itself** (i.e., symmetric around its center).

A tree is symmetric if:

```text
Left subtree is a mirror reflection of right subtree
```

---

## 📌 Example

#### Example 1 (Symmetric)

```
        1
       / \
      2   2
     / \ / \
    3  4 4  3
```

Output:

```
true
```

---

#### Example 2 (Not Symmetric)

```
        1
       / \
      2   2
       \     \
        3      3
```

Output:

```
false
```

---

## ✅ Solutions

---

## 🔹 1. Recursive Approach (Most Common)

---

### 💡 Idea

Two trees are mirror if:

1. Both NULL → true
2. One NULL → false
3. Values equal
4. Left subtree of one == Right subtree of other
5. Right subtree of one == Left subtree of other

---

### 🔹 Mirror Condition

```
isMirror(t1, t2):
    t1->val == t2->val
    AND
    isMirror(t1->left, t2->right)
    AND
    isMirror(t1->right, t2->left)
```

---

### 💻 C++ Code (Recursive + Memory Safe)

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
    bool isMirror(TreeNode* t1, TreeNode* t2) {
        if (!t1 && !t2) return true;
        if (!t1 || !t2) return false;

        return (t1->val == t2->val) &&
               isMirror(t1->left, t2->right) &&
               isMirror(t1->right, t2->left);
    }

public:
    bool isSymmetric(TreeNode* root) {
        if (!root) return true;
        return isMirror(root->left, root->right);
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
          2   2
         / \ / \
        3  4 4  3
    */

    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(2);

    root->left->left = new TreeNode(3);
    root->left->right = new TreeNode(4);

    root->right->left = new TreeNode(4);
    root->right->right = new TreeNode(3);

    Solution sol;
    cout << (sol.isSymmetric(root) ? "true" : "false") << endl;

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

## 🔹 2. Iterative Approach (Using Queue)

---

### 💡 Idea

Instead of recursion:

1. Push `(root->left, root->right)` into queue.
2. While queue not empty:

   * Compare pair.
   * Push `(left->left, right->right)`
   * Push `(left->right, right->left)`

---

### 💻 C++ Code (Iterative + Memory Safe)

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
    bool isSymmetric(TreeNode* root) {
        if (!root) return true;

        queue<TreeNode*> q;
        q.push(root->left);
        q.push(root->right);

        while (!q.empty()) {
            TreeNode* t1 = q.front(); q.pop();
            TreeNode* t2 = q.front(); q.pop();

            if (!t1 && !t2) continue;
            if (!t1 || !t2) return false;
            if (t1->val != t2->val) return false;

            q.push(t1->left);
            q.push(t2->right);

            q.push(t1->right);
            q.push(t2->left);
        }

        return true;
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
    root->right = new TreeNode(2);

    root->left->left = new TreeNode(3);
    root->left->right = new TreeNode(4);

    root->right->left = new TreeNode(4);
    root->right->right = new TreeNode(3);

    Solution sol;
    cout << (sol.isSymmetric(root) ? "true" : "false") << endl;

    deleteTree(root);
    return 0;
}
```

---

### ⏱ Time Complexity

* **O(n)**

### 🧠 Space Complexity

* Queue stores up to one level → **O(n)**

---

## 🔥 Final Comparison

| Approach  | Time | Space | Interview Preference |
| --------- | ---- | ----- | -------------------- |
| Recursive | O(n) | O(h)  | Most common          |
| Iterative | O(n) | O(n)  | Alternative          |

---

