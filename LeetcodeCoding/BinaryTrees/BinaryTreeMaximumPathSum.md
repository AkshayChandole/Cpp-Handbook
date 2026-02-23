
# [Binary Tree Maximum Path Sum](#binary-tree-maximum-path-sum)



## 📌 Problem Definition

Given the `root` of a binary tree, return the **maximum path sum**.

#### Path Definition:

* A path is any sequence of nodes connected by edges.
* The path:

  * Does **not** need to pass through the root.
  * Must contain at least one node.
  * Cannot revisit nodes.

#### Important:

Path sum is calculated using **node values** (not edges).

---

## 📌 Examples

#### Example 1

```
        1
       / \
      2   3
```

Output:

```
6
```

Explanation:

Best path: `2 → 1 → 3`
Sum = 2 + 1 + 3 = **6**

---

#### Example 2

```
        -10
        /  \
       9    20
           /  \
          15   7
```

Output:

```
42
```

Explanation:

Best path: `15 → 20 → 7`
Sum = 15 + 20 + 7 = **42**

---

## ✅ Solutions

---

## 🔹 1. Brute Force (Conceptual / Not Practical)

---

### 💡 Idea

For every node:

1. Consider it as highest point.
2. Compute maximum path in left subtree.
3. Compute maximum path in right subtree.
4. Try all combinations.

⚠️ This becomes exponential because:

* Many overlapping subproblems
* Recomputing subtree results repeatedly

Time complexity → **O(2ⁿ)** (not acceptable)

So we move directly to optimized DP on trees.

---

## 🔹 2. Optimal Approach (Bottom-Up DFS)

---

### 💡 Core Insight

At each node, two different values matter:

#### 1️⃣ Value to Return to Parent

A path going upward must be **single-branch only**:

```
node + max(leftGain, rightGain)
```

You cannot take both sides when returning to parent.

---

#### 2️⃣ Local Maximum Through Node

This considers both children:

```
node + leftGain + rightGain
```

This might form the global maximum path.

---

### 💡 Handle Negative Values

If subtree gives negative contribution:

```
max(0, leftGain)
max(0, rightGain)
```

We ignore negative paths.

---

### 🔹 Algorithm

```
gain(node):
    if node == NULL → return 0

    leftGain = max(0, gain(node->left))
    rightGain = max(0, gain(node->right))

    currentPathSum = node->val + leftGain + rightGain

    update globalMax

    return node->val + max(leftGain, rightGain)
```

---

## 💻 C++ Code (Optimal + Memory Safe)

```cpp
#include <iostream>
#include <algorithm>
#include <climits>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
private:
    int globalMax = INT_MIN;

    int maxGain(TreeNode* root) {
        if (!root) return 0;

        // Ignore negative contributions
        int leftGain = max(0, maxGain(root->left));
        int rightGain = max(0, maxGain(root->right));

        // Price of path passing through this node
        int currentPath = root->val + leftGain + rightGain;

        // Update global maximum
        globalMax = max(globalMax, currentPath);

        // Return gain for parent (single branch only)
        return root->val + max(leftGain, rightGain);
    }

public:
    int maxPathSum(TreeNode* root) {
        maxGain(root);
        return globalMax;
    }
};

// Proper deletion (Postorder cleanup)
void deleteTree(TreeNode* root) {
    if (!root) return;
    deleteTree(root->left);
    deleteTree(root->right);
    delete root;
}

int main() {

    /*
            -10
            /  \
           9    20
               /  \
              15   7
    */

    TreeNode* root = new TreeNode(-10);
    root->left = new TreeNode(9);
    root->right = new TreeNode(20);
    root->right->left = new TreeNode(15);
    root->right->right = new TreeNode(7);

    Solution sol;
    cout << sol.maxPathSum(root) << endl;  // Expected: 42

    deleteTree(root);  // ✅ Prevent memory leak
    return 0;
}
```

---

## ⏱ Time Complexity

* Each node visited once → **O(n)**

---

## 🧠 Space Complexity

* Recursive stack → **O(h)**

  * Worst case → **O(n)**
  * Balanced → **O(log n)**

---
