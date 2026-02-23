
# [Maximum Width of Binary Tree](#maximum-width-of-binary-tree)


## 📌 Problem Definition

Given the `root` of a binary tree, return the **maximum width** of the tree.

#### Width Definition:

The width of one level is defined as:

```text
Rightmost position index - Leftmost position index + 1
```

👉 Even if there are NULL nodes between them, they count in width calculation.

---

## 📌 Example

#### Example 1

```text
        1
       / \
      3   2
     / \   \
    5   3   9
```

Output:

```text
4
```

Explanation:

At level 3, positions are:

```
5 (index 0), 3 (1), null (2), 9 (3)
Width = 3 - 0 + 1 = 4
```

---

## 🔥 Core Idea

We simulate a **complete binary tree indexing system**:

For any node with index `i`:

```text
Left child  → 2*i
Right child → 2*i + 1
```

We use BFS and assign indices.

---

## ⚠️ Important Concern

Indices can grow very large (2^height).

To prevent overflow:

👉 Normalize indices at each level:

```text
Subtract minimum index of that level
```

---

## ✅ Optimal Approach (BFS + Indexing)

---

## 💻 C++ Code (Overflow Safe + Memory Clean)

```cpp
#include <iostream>
#include <queue>
#include <utility>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    int widthOfBinaryTree(TreeNode* root) {
        if (!root) return 0;

        long long maxWidth = 0;

        queue<pair<TreeNode*, long long>> q;
        q.push({root, 0});

        while (!q.empty()) {
            int size = q.size();

            long long minIndex = q.front().second;  // normalize
            long long first = 0, last = 0;

            for (int i = 0; i < size; i++) {
                auto [node, index] = q.front();
                q.pop();

                index -= minIndex;  // normalization

                if (i == 0) first = index;
                if (i == size - 1) last = index;

                if (node->left)
                    q.push({node->left, 2 * index});

                if (node->right)
                    q.push({node->right, 2 * index + 1});
            }

            maxWidth = max(maxWidth, last - first + 1);
        }

        return (int)maxWidth;
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
          3   2
         / \   \
        5   3   9
    */

    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(3);
    root->right = new TreeNode(2);
    root->left->left = new TreeNode(5);
    root->left->right = new TreeNode(3);
    root->right->right = new TreeNode(9);

    Solution sol;
    cout << sol.widthOfBinaryTree(root) << endl;

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

Queue stores one level:

```
O(n)
```

---

## 🔥 Why Normalization Is Necessary?

Without:

```cpp
index -= minIndex;
```

If tree depth = 1000:

```
Index ≈ 2^1000
```

Overflow occurs even with `long long`.

Normalization keeps numbers small.

---

## 🔥 Interview Insight

This problem tests:

* BFS mastery
* Index simulation
* Complete tree properties
* Overflow handling
* Level-based calculations

---

## 🔥 Common Mistakes

❌ Using int instead of long long
❌ Forgetting normalization
❌ Using node count instead of index difference
❌ Not considering null gaps

---

## 🔥 Pattern Used In

* Complete tree problems
* Heap-like indexing
* Level traversal problems
* Width / span calculations

---

## 🔥 Final Summary

| Technique   | Time | Space | Difficulty  |
| ----------- | ---- | ----- | ----------- |
| BFS + Index | O(n) | O(n)  | Medium      |
| DFS + Map   | O(n) | O(n)  | Alternative |

---

