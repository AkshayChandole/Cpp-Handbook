
# [Construct Binary Tree from Preorder and Inorder Traversal](#construct-binary-tree-from-preorder-and-inorder-traversal)

## 📌 Problem Definition

Given two integer arrays:

* `preorder`
* `inorder`

Construct and return the binary tree.

#### Assumptions:

* All values are unique.
* Both traversals are valid.
* The tree can be uniquely reconstructed.

---

## 📌 Traversal Properties

#### Preorder

```text
Root → Left → Right
```

#### Inorder

```text
Left → Root → Right
```

---

## 📌 Example

#### Example

```text
preorder = [3,9,20,15,7]
inorder  = [9,3,15,20,7]
```

Constructed tree:

```text
        3
       / \
      9   20
         /  \
        15   7
```

---

## 🔥 Core Insight

#### Important Observations

1️⃣ First element in preorder is always **root**.

```text
preorder[preIndex] = root
```

2️⃣ Find root in inorder array → split into:

* Left subtree
* Right subtree

3️⃣ Unlike postorder case, here we must:

```text
Build LEFT subtree first
Then RIGHT subtree
```

Because preorder order is:

```text
Root → Left → Right
```

---

## ✅ Solution (Optimal – O(n))

---

## 🔹 Algorithm

1. Maintain `preIndex = 0`
2. Use hashmap to store inorder indices
3. Recursively:

   * Pick preorder[preIndex] → root
   * Increment preIndex
   * Build left subtree
   * Build right subtree

---

## 💻 C++ Code (Optimized + Memory Safe)

```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
private:
    int preIndex;
    unordered_map<int, int> inorderMap;

    TreeNode* build(vector<int>& preorder,
                    int inStart, int inEnd) {

        if (inStart > inEnd)
            return nullptr;

        // Pick current root
        int rootVal = preorder[preIndex++];
        TreeNode* root = new TreeNode(rootVal);

        // Find root index in inorder
        int inRootIndex = inorderMap[rootVal];

        // Build Left subtree first
        root->left = build(preorder,
                           inStart, inRootIndex - 1);

        root->right = build(preorder,
                            inRootIndex + 1, inEnd);

        return root;
    }

public:
    TreeNode* buildTree(vector<int>& preorder,
                        vector<int>& inorder) {

        preIndex = 0;

        for (int i = 0; i < inorder.size(); i++)
            inorderMap[inorder[i]] = i;

        return build(preorder,
                     0, inorder.size() - 1);
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

    vector<int> preorder = {3,9,20,15,7};
    vector<int> inorder  = {9,3,15,20,7};

    Solution sol;
    TreeNode* root = sol.buildTree(preorder, inorder);

    cout << "Root value: " << root->val << endl;

    deleteTree(root);
    return 0;
}
```

---

## ⏱ Time Complexity

* Building hashmap → O(n)
* Each node created once → O(n)

```text
Total = O(n)
```

---

## 🧠 Space Complexity

* Hashmap → O(n)
* Recursion stack → O(h)

Worst case (skewed tree):

```text
O(n)
```

Balanced tree:

```text
O(log n)
```

---

## 🔥 Why Build Left Subtree First?

Because preorder is:

```text
Root → Left → Right
```

So after selecting root:

* Next element must belong to left subtree.
* Then remaining elements belong to right subtree.

If you build right first → tree structure breaks.

---

## 🔥 Comparison with Postorder + Inorder

| Case                | Root Comes From   | Build Order  |
| ------------------- | ----------------- | ------------ |
| Preorder + Inorder  | Start of preorder | Left → Right |
| Postorder + Inorder | End of postorder  | Right → Left |

---

## 🔥 Common Interview Mistakes

❌ Forgetting to use hashmap → leads to O(n²)
❌ Incorrect index boundaries
❌ Using global index incorrectly
❌ Mixing left/right build order

---
