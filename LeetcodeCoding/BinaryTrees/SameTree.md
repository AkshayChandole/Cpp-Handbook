# [Same Tree]()


## 📌 Problem Definition

Given two binary trees `p` and `q`, write a function to check whether they are the **same**.

Two trees are considered the same if:

1. They are structurally identical.
2. The nodes have the same values.

---

## 📌 Examples

#### Example 1 (Same)

```
    1        1
   / \      / \
  2   3    2   3
```

Output:

```
true
```

---

#### Example 2 (Different Structure)

```
    1        1
   /          \
  2            2
```

Output:

```
false
```

---

#### Example 3 (Different Values)

```
    1        1
   / \      / \
  2   1    1   2
```

Output:

```
false
```

---

## ✅ Solutions

---

## 🔹 1. Brute Force (Recursive DFS)

---

### 💡 Idea

For two nodes to be same:

```
1. Both are NULL → true
2. One is NULL → false
3. Values must match
4. Left subtrees must match
5. Right subtrees must match
```

---

### 💻 C++ Code (Recursive + Proper Cleanup)

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
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (!p && !q) return true;
        if (!p || !q) return false;
        if (p->val != q->val) return false;

        return isSameTree(p->left, q->left) &&
               isSameTree(p->right, q->right);
    }
};

// Proper deletion
void deleteTree(TreeNode* root) {
    if (!root) return;
    deleteTree(root->left);
    deleteTree(root->right);
    delete root;
}

int main() {

    // Tree 1
    TreeNode* p = new TreeNode(1);
    p->left = new TreeNode(2);
    p->right = new TreeNode(3);

    // Tree 2
    TreeNode* q = new TreeNode(1);
    q->left = new TreeNode(2);
    q->right = new TreeNode(3);

    Solution sol;
    cout << (sol.isSameTree(p, q) ? "true" : "false") << endl;

    deleteTree(p);
    deleteTree(q);

    return 0;
}
```

---

### ⏱ Time Complexity

* Each node compared once → **O(n)**
  (n = number of nodes in smaller tree)

---

### 🧠 Space Complexity

* Recursive stack depth → **O(h)**

  * Worst case (skewed tree) → **O(n)**
  * Balanced tree → **O(log n)**

---

## 🔹 2. Iterative Approach (Using Queue – BFS)

---

### 💡 Idea

Use queue to compare nodes level-by-level.

Steps:

1. Push both roots into queue.
2. While queue not empty:

   * Pop two nodes.
   * Compare values.
   * Push left children pair.
   * Push right children pair.

---

### 💻 C++ Code (Iterative + Cleanup)

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
    bool isSameTree(TreeNode* p, TreeNode* q) {
        queue<TreeNode*> que;
        que.push(p);
        que.push(q);

        while (!que.empty()) {
            TreeNode* node1 = que.front(); que.pop();
            TreeNode* node2 = que.front(); que.pop();

            if (!node1 && !node2) continue;
            if (!node1 || !node2) return false;
            if (node1->val != node2->val) return false;

            que.push(node1->left);
            que.push(node2->left);
            que.push(node1->right);
            que.push(node2->right);
        }

        return true;
    }
};

void deleteTree(TreeNode* root) {
    if (!root) return;
    deleteTree(root->left);
    deleteTree(root->right);
    delete root;
}

int main() {

    TreeNode* p = new TreeNode(1);
    p->left = new TreeNode(2);
    p->right = new TreeNode(3);

    TreeNode* q = new TreeNode(1);
    q->left = new TreeNode(2);
    q->right = new TreeNode(3);

    Solution sol;
    cout << (sol.isSameTree(p, q) ? "true" : "false") << endl;

    deleteTree(p);
    deleteTree(q);

    return 0;
}
```

---

### ⏱ Time Complexity

* Each node visited once → **O(n)**

---

### 🧠 Space Complexity

* Queue stores up to one level → **O(n)** worst case

---

## 🔥 Final Comparison

| Approach      | Time | Space | Interview Preference |
| ------------- | ---- | ----- | -------------------- |
| Recursive     | O(n) | O(h)  | Most common          |
| Iterative BFS | O(n) | O(n)  | Alternative          |

---



