
# [Count Complete Tree Nodes](#count-complete-tree-nodes)

## 📌 Problem Definition

Given the `root` of a **complete binary tree**, return the number of nodes.

#### Complete Binary Tree Definition:

* Every level is completely filled except possibly the last.
* Last level nodes are filled from left to right.

---

## 📌 Example

#### Example 1

```text
        1
       / \
      2   3
     / \  /
    4  5 6
```

Output:

```text
6
```

---

## 🔥 Important Observation

In a complete tree:

* If left height == right height → tree is **perfect**
* Number of nodes in perfect tree:

```text
2^h - 1
```

This helps us avoid traversing entire tree.

---

## ✅ Solutions

---

## 🔹 1. Brute Force (Normal DFS Count)

---

### 💡 Idea

Traverse entire tree and count nodes.

---

### 💻 C++ Code (Simple DFS + Cleanup)

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
    int countNodes(TreeNode* root) {
        if (!root) return 0;
        return 1 + countNodes(root->left) + countNodes(root->right);
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
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);
    root->right->left = new TreeNode(6);

    Solution sol;
    cout << sol.countNodes(root) << endl;

    deleteTree(root);
    return 0;
}
```

---

### ⏱ Time Complexity

```
O(n)
```

### 🧠 Space Complexity

```
O(h)
```

---

## 🔹 2. Optimal Approach (Using Complete Tree Property)

---

### 💡 Core Idea

For every subtree:

1️⃣ Compute leftmost height
2️⃣ Compute rightmost height

If equal → subtree is perfect → use formula.

Otherwise → recursively count.

---

### 🔹 Height Calculation

```text
Left height → keep going left
Right height → keep going right
```

---

### 💻 C++ Code (Optimized O(log² n) + Cleanup)

```cpp
#include <iostream>
#include <cmath>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
private:
    int leftHeight(TreeNode* node) {
        int height = 0;
        while (node) {
            height++;
            node = node->left;
        }
        return height;
    }

    int rightHeight(TreeNode* node) {
        int height = 0;
        while (node) {
            height++;
            node = node->right;
        }
        return height;
    }

public:
    int countNodes(TreeNode* root) {
        if (!root) return 0;

        int lh = leftHeight(root);
        int rh = rightHeight(root);

        if (lh == rh) {
            // Perfect binary tree
            return pow(2, lh) - 1;
        }

        return 1 + countNodes(root->left)
                 + countNodes(root->right);
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
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);
    root->right->left = new TreeNode(6);

    Solution sol;
    cout << sol.countNodes(root) << endl;

    deleteTree(root);
    return 0;
}
```

---

## ⏱ Time Complexity

Height calculation takes:

```
O(log n)
```

We do it for each recursive call:

```
O(log n)
```

Total:

```
O(log² n)
```

---

## 🧠 Space Complexity

Recursion stack:

```
O(log n)
```

---

## 🔥 Why This Is Faster?

Because:

* Instead of visiting all nodes
* We skip entire perfect subtrees in O(1) time using formula.

---

## 🔥 Even More Optimized Approach (Binary Search on Last Level)

Advanced version:

* Compute tree height
* Binary search number of nodes on last level

Time:

```
O(log² n)
```

(Same complexity but more mathematical approach)

---

## 🔥 Interview Insight

This problem tests:

* Understanding of complete tree structure
* Logarithmic thinking
* Avoiding full traversal
* Mathematical observation

---

## 🔥 Final Comparison

| Approach      | Time      | Space    | Interview Level |
| ------------- | --------- | -------- | --------------- |
| DFS           | O(n)      | O(h)     | Basic           |
| Height Trick  | O(log² n) | O(log n) | Expected        |
| Binary Search | O(log² n) | O(1)     | Advanced        |

---

