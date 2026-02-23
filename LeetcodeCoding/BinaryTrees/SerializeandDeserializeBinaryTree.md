
# [Serialize and Deserialize Binary Tree](#serialize-and-deserialize-binary-tree)


## 📌 Problem Definition

Design an algorithm to:

* **Serialize** a binary tree → convert it into a string.
* **Deserialize** the string back into the original binary tree.

#### Requirements:

* Preserve structure.
* Preserve values.
* Handle `NULL` nodes properly.

---

## 📌 Example

#### Example

```id="w4xthg"
        1
       / \
      2   3
         / \
        4   5
```

Serialized form (one possible representation):

```id="m5tyqx"
"1,2,3,null,null,4,5,null,null,null,null"
```

After deserialization → original tree reconstructed.

---

## ✅ Solutions

---

## 🔹 1. Brute Force Idea (Without NULL markers ❌ Wrong Approach)

If we serialize only values using preorder:

```text
1,2,3,4,5
```

We lose structure information.

⚠️ Cannot reconstruct tree uniquely.

So we must include `NULL` markers.

---

## 🔹 2. Optimal Approach (Preorder DFS with NULL markers)

---

### 💡 Key Idea

Use preorder traversal:

```text id="sh95xw"
Root → Left → Right
```

For null nodes:

```text id="7r3ztf"
Add "null"
```

Example:

```text id="21w0iy"
1,2,null,null,3,4,null,null,5,null,null
```

This guarantees full reconstruction.

---

## 🔹 Serialization Algorithm

```text id="i98mtt"
serialize(node):
    if node == NULL:
        append "null,"
        return
    append value + ","
    serialize(left)
    serialize(right)
```

---

## 🔹 Deserialization Algorithm

Read tokens one by one:

```text id="4b8av3"
deserialize():
    if token == "null":
        return NULL
    create node
    node->left = deserialize()
    node->right = deserialize()
```

---

## 💻 C++ Code (DFS + Memory Safe)

```cpp
#include <iostream>
#include <sstream>
#include <string>
using namespace std;

struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class Codec {
public:

    // Serialize
    void serializeHelper(TreeNode* root, string& result) {
        if (!root) {
            result += "null,";
            return;
        }

        result += to_string(root->val) + ",";
        serializeHelper(root->left, result);
        serializeHelper(root->right, result);
    }

    string serialize(TreeNode* root) {
        string result;
        serializeHelper(root, result);
        return result;
    }

    // Deserialize
    TreeNode* deserializeHelper(stringstream& ss) {
        string token;
        getline(ss, token, ',');

        if (token == "null")
            return nullptr;

        TreeNode* node = new TreeNode(stoi(token));
        node->left = deserializeHelper(ss);
        node->right = deserializeHelper(ss);

        return node;
    }

    TreeNode* deserialize(string data) {
        stringstream ss(data);
        return deserializeHelper(ss);
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
          2   3
             / \
            4   5
    */

    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->right->left = new TreeNode(4);
    root->right->right = new TreeNode(5);

    Codec codec;

    // Serialize
    string serialized = codec.serialize(root);
    cout << "Serialized: " << serialized << endl;

    // Deserialize
    TreeNode* newRoot = codec.deserialize(serialized);

    cout << "Root after deserialize: " << newRoot->val << endl;

    deleteTree(root);
    deleteTree(newRoot);

    return 0;
}
```

---

## ⏱ Time Complexity

#### Serialize:

* Each node visited once → **O(n)**

#### Deserialize:

* Each token processed once → **O(n)**

---

## 🧠 Space Complexity

* Recursive stack → **O(h)**
* String storage → **O(n)**

---

## 🔥 Alternative Approach (BFS Level Order)

We can also:

1. Use queue
2. Serialize level-by-level
3. Insert `"null"` for missing children

Time: O(n)
Space: O(n)

This approach is sometimes preferred in interviews.

---
