
# [All Nodes Distance K in Binary Tree](#all-nodes-distance-k-in-binary-tree)



## 📌 Problem Definition

Given:

* `root` of a binary tree
* A `target` node
* An integer `k`

Return all node values that are exactly **distance K** from the target node.

#### Distance:

Number of edges between two nodes.

---

## 📌 Example

#### Example

```
            3
           / \
          5   1
         / \  / \
        6  2 0   8
          / \
         7   4
```

Target = 5
K = 2

Output:

```
[7,4,1]
```

Explanation:

* 7 (2 edges away)
* 4 (2 edges away)
* 1 (2 edges away)

---

## 🔥 Key Challenge

Tree is NOT bidirectional.

From target:

* We can go left
* We can go right
* But we cannot go to parent directly

So we must convert tree into **undirected graph behavior**.

---

## ✅ Optimal Approach (BFS from Target)

---

## 🔹 Step 1: Build Parent Mapping

Traverse tree and store:

```
child → parent
```

Use:

```cpp
unordered_map<TreeNode*, TreeNode*>
```

---

## 🔹 Step 2: BFS from Target

* Use queue
* Use visited set
* Traverse in 3 directions:

  * left
  * right
  * parent
* Stop when level == k

---

## 💻 C++ Code (Optimized + Memory Safe)

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <unordered_map>
#include <unordered_set>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Solution {
private:
    void buildParentMap(TreeNode* root,
                        unordered_map<TreeNode*, TreeNode*>& parent) {

        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {
            TreeNode* node = q.front();
            q.pop();

            if (node->left) {
                parent[node->left] = node;
                q.push(node->left);
            }

            if (node->right) {
                parent[node->right] = node;
                q.push(node->right);
            }
        }
    }

public:
    vector<int> distanceK(TreeNode* root,
                          TreeNode* target,
                          int k) {

        unordered_map<TreeNode*, TreeNode*> parent;
        buildParentMap(root, parent);

        unordered_set<TreeNode*> visited;
        queue<TreeNode*> q;

        q.push(target);
        visited.insert(target);

        int level = 0;

        while (!q.empty()) {

            int size = q.size();

            if (level == k)
                break;

            level++;

            for (int i = 0; i < size; i++) {
                TreeNode* node = q.front();
                q.pop();

                // Left
                if (node->left &&
                    !visited.count(node->left)) {
                    visited.insert(node->left);
                    q.push(node->left);
                }

                // Right
                if (node->right &&
                    !visited.count(node->right)) {
                    visited.insert(node->right);
                    q.push(node->right);
                }

                // Parent
                if (parent.count(node) &&
                    !visited.count(parent[node])) {
                    visited.insert(parent[node]);
                    q.push(parent[node]);
                }
            }
        }

        vector<int> result;
        while (!q.empty()) {
            result.push_back(q.front()->val);
            q.pop();
        }

        return result;
    }
};


// Cleanup
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

    TreeNode* target = root->left; // node 5
    int k = 2;

    vector<int> result = sol.distanceK(root, target, k);

    for (int val : result)
        cout << val << " ";

    deleteTree(root);
    return 0;
}
```

---

## ⏱ Time Complexity

1️⃣ Build parent map → O(n)
2️⃣ BFS traversal → O(n)

```
Total = O(n)
```

---

## 🧠 Space Complexity

* Parent map → O(n)
* Visited set → O(n)
* Queue → O(n)

```
O(n)
```

---

## 🔥 Why BFS Instead of DFS?

Because:

We need nodes at **exact distance K**.

BFS naturally explores:

```
Level 0
Level 1
Level 2
...
```

So when level == K → we stop.

DFS would require tracking distance manually.

---
