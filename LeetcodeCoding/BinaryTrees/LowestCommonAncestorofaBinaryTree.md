
# [Lowest Common Ancestor of a Binary Tree](#lowest-common-ancestor-of-a-binary-tree)


## 📌 Problem Definition

Given:

* `root` of a binary tree
* Two nodes `p` and `q`

Return their **Lowest Common Ancestor (LCA)**.

#### Definition:

The LCA of two nodes `p` and `q` is the **lowest node in the tree that has both p and q as descendants**
(a node can be a descendant of itself).

---

## 📌 Example

```text
            3
           / \
          5   1
         / \  / \
        6  2 0   8
          / \
         7   4
```

#### Case 1:

```
p = 5
q = 1
```

Output:

```
3
```

---

#### Case 2:

```
p = 5
q = 4
```

Output:

```
5
```

(5 is ancestor of 4)

---

## 🔥 Core Idea (Most Important Understanding)

At every node:

* If both `p` and `q` are found in different subtrees → current node is LCA.
* If both are found in one subtree → LCA is inside that subtree.
* If current node is either `p` or `q` → return it.

---

## ✅ Optimal Recursive Approach (Single Pass)

---

## 🔹 Algorithm Logic

```text
If root == NULL → return NULL

If root == p OR root == q:
    return root

left = LCA(root->left)
right = LCA(root->right)

If left != NULL AND right != NULL:
    return root   // split found

If left != NULL:
    return left

If right != NULL:
    return right

return NULL
```

---

## 💻 C++ Code (Optimized + Memory Safe)

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
    TreeNode* lowestCommonAncestor(TreeNode* root,
                                   TreeNode* p,
                                   TreeNode* q) {

        if (!root || root == p || root == q)
            return root;

        TreeNode* left =
            lowestCommonAncestor(root->left, p, q);

        TreeNode* right =
            lowestCommonAncestor(root->right, p, q);

        // If both sides return non-null → this is LCA
        if (left && right)
            return root;

        // Otherwise return non-null side
        return left ? left : right;
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
            3
           / \
          5   1
         / \  / \
        6  2 0   8
          / \
         7   4
    */

    TreeNode* root = new TreeNode(3);
    root->left = new TreeNode(5);
    root->right = new TreeNode(1);

    root->left->left = new TreeNode(6);
    root->left->right = new TreeNode(2);
    root->right->left = new TreeNode(0);
    root->right->right = new TreeNode(8);

    root->left->right->left = new TreeNode(7);
    root->left->right->right = new TreeNode(4);

    Solution sol;

    TreeNode* p = root->left;          // 5
    TreeNode* q = root->right;         // 1

    TreeNode* lca =
        sol.lowestCommonAncestor(root, p, q);

    cout << "LCA: " << lca->val << endl;

    deleteTree(root);
    return 0;
}
```

---

## ⏱ Time Complexity

Each node visited once:

```
O(n)
```

---

## 🧠 Space Complexity

Recursive stack depth:

Worst case (skewed):

```
O(n)
```

Balanced:

```
O(log n)
```

---

## 🔥 Why This Works (Intuition)

Think bottom-up:

* If a subtree contains both `p` and `q`,
* That subtree root becomes answer.

When recursion returns:

| Left Result | Right Result | Meaning                |
| ----------- | ------------ | ---------------------- |
| NULL        | NULL         | No target here         |
| Node        | NULL         | Found in left          |
| NULL        | Node         | Found in right         |
| Node        | Node         | Split → current is LCA |

---

## 🔥 Interview Insights

This problem tests:

* Tree recursion mastery
* Divide & conquer thinking
* Post-order traversal logic
* Bottom-up reasoning

---

## 🔥 Important Special Case

If:

```
p is ancestor of q
```

Then algorithm correctly returns `p`.

Because:

```cpp
if (root == p || root == q)
    return root;
```

---

## 🔥 Alternative Approach (Using Parent Map)

1. Build parent mapping
2. Store ancestors of p
3. Traverse ancestors of q
4. First match is LCA

Time: O(n)
Space: O(n)

But recursive approach is cleaner and preferred.

---

## 🔥 Difference From BST LCA

In Binary Search Tree:

We use ordering property:

```cpp
if (p->val < root->val && q->val < root->val)
    go left;
```

But here:

❌ No ordering guarantee
So we must explore both sides.

---

## 🎯 Final Summary

| Approach      | Time | Space | Preference  |
| ------------- | ---- | ----- | ----------- |
| Recursive DFS | O(n) | O(h)  | ⭐ Best      |
| Parent Map    | O(n) | O(n)  | Alternative |

---

