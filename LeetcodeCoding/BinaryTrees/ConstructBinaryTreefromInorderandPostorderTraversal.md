
# [Construct Binary Tree from Inorder and Postorder Traversal](#construct-binary-tree-from-inorder-and-postorder-traversal)

## 📌 Problem Definition

Given two integer arrays:

* `inorder`
* `postorder`

Construct and return the binary tree.

#### Assumptions:

* All values are unique.
* Both traversals are valid.
* Tree can be reconstructed uniquely.

---

## 📌 Key Traversal Properties

#### Inorder

```text
Left → Root → Right
```

#### Postorder

```text
Left → Right → Root
```

---

## 📌 Example

#### Example

```text
inorder   = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

Constructed tree:

```text id="eq9g0h"
        3
       / \
      9   20
         /  \
        15   7
```

---

## 🔥 Core Insight

#### Important Observations

1️⃣ Last element in postorder is **root**.

```text
postorder.back() = root
```

2️⃣ Find root in inorder → divide into:

* Left subtree
* Right subtree

3️⃣ Very Important:

Because postorder is:

```text
Left → Right → Root
```

When building tree recursively:

👉 You must build **Right subtree first**, then Left subtree.

Why?

Because we process postorder from end.

---

## ✅ Solution (Optimal – O(n))

---

## 🔹 Algorithm

1. Maintain index `postIndex = n - 1`
2. Use hashmap to store inorder indices
3. Recursively:

   * Pick postorder[postIndex] → root
   * Decrement postIndex
   * Build right subtree
   * Build left subtree

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
    int postIndex;
    unordered_map<int, int> inorderMap;

    TreeNode* build(vector<int>& inorder, vector<int>& postorder,
                    int inStart, int inEnd) {

        if (inStart > inEnd)
            return nullptr;

        // Pick current root
        int rootVal = postorder[postIndex--];
        TreeNode* root = new TreeNode(rootVal);

        // Find root index in inorder
        int inRootIndex = inorderMap[rootVal];

        // IMPORTANT: Build Right first
        root->right = build(inorder, postorder,
                            inRootIndex + 1, inEnd);

        root->left = build(inorder, postorder,
                           inStart, inRootIndex - 1);

        return root;
    }

public:
    TreeNode* buildTree(vector<int>& inorder,
                        vector<int>& postorder) {

        postIndex = postorder.size() - 1;

        for (int i = 0; i < inorder.size(); i++)
            inorderMap[inorder[i]] = i;

        return build(inorder, postorder,
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

    vector<int> inorder = {9,3,15,20,7};
    vector<int> postorder = {9,15,7,20,3};

    Solution sol;
    TreeNode* root = sol.buildTree(inorder, postorder);

    cout << "Root value: " << root->val << endl;

    deleteTree(root);
    return 0;
}
```

---

## ⏱ Time Complexity

#### With HashMap:

* Building map → O(n)
* Each node processed once → O(n)

```text
Total = O(n)
```

---

## 🧠 Space Complexity

* HashMap → O(n)
* Recursion stack → O(h)

Worst case:

```text
O(n)
```

Balanced tree:

```text
O(log n)
```

---

## 🔥 Why Build Right Subtree First?

Because we are reading postorder from back.

Postorder format:

```text
Left → Right → Root
```

Reversed postorder:

```text
Root → Right → Left
```

So recursion must follow:

```text
Root
Right subtree
Left subtree
```

If you build left first → structure becomes wrong.

---

## 🔥 Common Interview Mistakes

❌ Forgetting to build right first
❌ Not using hashmap → O(n²) solution
❌ Incorrect index boundaries
❌ Not decrementing postIndex properly

---
