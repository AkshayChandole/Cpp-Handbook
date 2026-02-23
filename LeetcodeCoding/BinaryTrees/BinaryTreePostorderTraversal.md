
# [Binary Tree Postorder Traversal](#binary-tree-postorder-traversal)


## 📌 Problem Definition

Given the root of a binary tree, return the **postorder traversal** of its nodes' values.

#### Postorder Traversal Order:

```
Left → Right → Root
```

---

## 📌 Examples

#### Example 1

Input:

```
    1
     \
      2
     /
    3
```

Output:

```
[3,2,1]
```

---

## ✅ Solutions

---

## 🔹 1. Brute Force (Recursive DFS)

---

### 💡 Approach

Postorder traversal using recursion:

1. Traverse left subtree
2. Traverse right subtree
3. Visit root

---

### 💻 C++ Code (Recursive + Proper Memory Cleanup)

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
public:
    void postorder(TreeNode* root, vector<int>& result) {
        if (!root) return;

        postorder(root->left, result);
        postorder(root->right, result);
        result.push_back(root->val);
    }

    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        postorder(root, result);
        return result;
    }
};

// Proper tree deletion (Postorder delete)
void deleteTree(TreeNode* root) {
    if (!root) return;
    deleteTree(root->left);
    deleteTree(root->right);
    delete root;
}

int main() {
    /*
            1
             \
              2
             /
            3
    */

    TreeNode* root = new TreeNode(1);
    root->right = new TreeNode(2);
    root->right->left = new TreeNode(3);

    Solution sol;
    vector<int> result = sol.postorderTraversal(root);

    for (int val : result)
        cout << val << " ";

    deleteTree(root);   // ✅ Prevent memory leak

    return 0;
}
```

---

### ⏱ Time Complexity

* Each node visited once → **O(n)**

### 🧠 Space Complexity

* Recursive stack → **O(h)**

  * Worst case (skewed tree) → **O(n)**
  * Balanced → **O(log n)**

---

## 🔹 2. Better Approach (Iterative Using Two Stacks)

---

### 💡 Approach

Use two stacks:

1. Push root into stack1
2. Pop from stack1 and push into stack2
3. Push left and right children into stack1
4. Finally, pop stack2 → gives postorder

Why?
Because reversing **Root → Right → Left** gives **Left → Right → Root**

---

### 💻 C++ Code (Two Stacks + Cleanup)

```cpp
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        if (!root) return result;

        stack<TreeNode*> st1, st2;
        st1.push(root);

        while (!st1.empty()) {
            TreeNode* node = st1.top();
            st1.pop();
            st2.push(node);

            if (node->left)
                st1.push(node->left);
            if (node->right)
                st1.push(node->right);
        }

        while (!st2.empty()) {
            result.push_back(st2.top()->val);
            st2.pop();
        }

        return result;
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
    root->right = new TreeNode(2);
    root->right->left = new TreeNode(3);

    Solution sol;
    vector<int> result = sol.postorderTraversal(root);

    for (int val : result)
        cout << val << " ";

    deleteTree(root);   // ✅ Memory cleanup

    return 0;
}
```

---

### ⏱ Time Complexity

* **O(n)**

### 🧠 Space Complexity

* Two stacks → **O(n)** worst case

---

## 🔹 3. Optimal Approach (Single Stack – Most Interview Favorite)

---

### 💡 Approach

Use one stack and track last visited node.

Algorithm:

1. Push all left nodes
2. Peek top node
3. If right child exists and not processed → move right
4. Else → process node

This mimics recursion manually.

---

### 💻 C++ Code (Single Stack + Cleanup)

```cpp
#include <iostream>
#include <vector>
#include <stack>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        TreeNode* curr = root;
        TreeNode* lastVisited = nullptr;

        while (curr || !st.empty()) {

            while (curr) {
                st.push(curr);
                curr = curr->left;
            }

            TreeNode* node = st.top();

            if (node->right && lastVisited != node->right) {
                curr = node->right;
            } else {
                result.push_back(node->val);
                lastVisited = node;
                st.pop();
            }
        }

        return result;
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
    root->right = new TreeNode(2);
    root->right->left = new TreeNode(3);

    Solution sol;
    vector<int> result = sol.postorderTraversal(root);

    for (int val : result)
        cout << val << " ";

    deleteTree(root);   // ✅ Memory cleanup

    return 0;
}
```

---

### ⏱ Time Complexity

* Each node processed once → **O(n)**

### 🧠 Space Complexity

* Stack up to height → **O(h)**

  * Worst case → **O(n)**

---

## 🔥 Final Comparison

| Approach     | Time | Space | Interview Depth  |
| ------------ | ---- | ----- | ---------------- |
| Recursive    | O(n) | O(h)  | Basic            |
| Two Stacks   | O(n) | O(n)  | Standard         |
| Single Stack | O(n) | O(h)  | Strong Candidate |

---

#### ⚡ Interview Insight

Postorder is hardest because:

* Root must be processed **after** both children
* That’s why tracking `lastVisited` is required

---

