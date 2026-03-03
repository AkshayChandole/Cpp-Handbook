# [Validate Binary Search Tree](#validate-binary-search-tree)

## 🟢 Validate Binary Search Tree

---

### 📌 Problem

Given the `root` of a binary tree, determine if it is a **valid Binary Search Tree (BST)**.

A BST is valid if:

1. Left subtree contains only nodes with values **less than** the node's value.
2. Right subtree contains only nodes with values **greater than** the node's value.
3. Both left and right subtrees must also be BSTs.

---

### 📌 Examples

```id="ex1"
Input:
    2
   / \
  1   3

Output: true
```

```id="ex2"
Input:
    5
   / \
  1   4
     / \
    3   6

Output: false
Explanation:
Node 3 is in right subtree of 5 but 3 < 5.
```

---

# 🔴 1. Brute Force (Check Immediate Children Only)

### 💡 Idea

Check:

* node->left < node
* node->right > node

Recursively validate subtrees.

---

### ❌ Why It Fails

It does NOT check entire subtree range.

Example:

```id="fail"
    5
   / \
  1   6
     / \
    3   7
```

3 < 5 but in right subtree → invalid.

Naive solution incorrectly returns true.

---

### 💻 Incorrect Code

```cpp
bool isValidBST(TreeNode* root) {
    if(!root) return true;

    if(root->left && root->left->val >= root->val)
        return false;
    if(root->right && root->right->val <= root->val)
        return false;

    return isValidBST(root->left) &&
           isValidBST(root->right);
}
```

❌ Wrong approach.

---

# 🟡 2. Better Solution (Range Validation)

### 💡 Key Insight

Each node must satisfy:

```id="range"
min < node->val < max
```

For:

* Left subtree → max becomes node->val
* Right subtree → min becomes node->val

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    bool validate(TreeNode* node,
                  long minVal,
                  long maxVal) {

        if(!node) return true;

        if(node->val <= minVal ||
           node->val >= maxVal)
            return false;

        return validate(node->left,
                        minVal,
                        node->val) &&
               validate(node->right,
                        node->val,
                        maxVal);
    }

    bool isValidBST(TreeNode* root) {
        return validate(root, LONG_MIN, LONG_MAX);
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(H) recursion stack

(H = tree height)

✅ Correct and efficient.

---

# 🟢 3. Optimum Solution (Inorder Traversal)

### 💡 Even Simpler Insight

Inorder traversal of BST must produce:

```id="sorted"
strictly increasing sequence
```

So:

* Perform inorder traversal.
* Check if current value > previous value.

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    long prev = LONG_MIN;

    bool inorder(TreeNode* root) {
        if(!root) return true;

        if(!inorder(root->left))
            return false;

        if(root->val <= prev)
            return false;

        prev = root->val;

        return inorder(root->right);
    }

    bool isValidBST(TreeNode* root) {
        return inorder(root);
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(H)

⭐ Most elegant solution.

---

# 🔍 Why Inorder Works

For BST:

```id="property"
Left → Node → Right
```

This guarantees sorted order.

If any violation occurs:

* BST property breaks.

---

# 🔥 Important Edge Case

Use:

```cpp
long prev = LONG_MIN;
```

Not `int`, because:

```id="overflow"
node->val can be INT_MIN
```

Using `int` may cause incorrect comparison.

---

# 🏆 Final Comparison

| Approach          | Time | Space | Recommended |
| ----------------- | ---- | ----- | ----------- |
| Child Check Only  | O(N) | O(H)  | ❌           |
| Range Validation  | O(N) | O(H)  | ✅           |
| Inorder Traversal | O(N) | O(H)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* Global constraints in recursion
* Range passing technique
* Inorder traversal property of BST

Related problems:

* Recover BST
* Kth Smallest in BST
* Lowest Common Ancestor in BST
* Convert BST to Sorted DLL

---
