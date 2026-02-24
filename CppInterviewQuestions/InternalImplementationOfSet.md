# [How is std::set implemented internally?](#How-is-stdset-implemented-internally)

### 🔹 std::set – Internal Implementation

`std::set` is internally implemented almost exactly like `std::map`.

The only difference:

> 🔹 `std::map` stores **key → value**
> 🔹 `std::set` stores **only keys**

Both are typically implemented using a **Red-Black Tree (self-balancing BST)**.

---

## 1️⃣ Core Data Structure – Red-Black Tree

Each node contains:

```cpp
template<typename Key>
struct Node {
    Key key;

    Node* parent;
    Node* left;
    Node* right;

    bool isRed;
};
```

Compared to `std::map`:

```cpp
std::pair<const Key, T>
```

Here:

```cpp
Key
```

Only key is stored.

---

## 2️⃣ Why Red-Black Tree?

Red-Black Tree guarantees:

```
Height <= 2 * log2(n + 1)
```

So operations are always:

| Operation | Complexity |
| --------- | ---------- |
| insert    | O(log n)   |
| erase     | O(log n)   |
| find      | O(log n)   |

---

## 3️⃣ Ordering Mechanism

By default, set uses:

```cpp
std::less<Key>
```

So tree ordering is based on comparator:

```cpp
if (comp(newKey, node->key))
    go left;
else
    go right;
```

You can customize:

```cpp
std::set<int, std::greater<int>>
```

Tree structure changes based on comparator.

---

## 4️⃣ Insertion Internals

#### Step 1 – BST Insert

Traverse tree:

```cpp
if (key < current->key)
    left
else if (key > current->key)
    right
else
    duplicate → ignore
```

Important:

> `std::set` does NOT allow duplicates

---

#### Step 2 – Insert as RED

New node inserted as red.

---

#### Step 3 – Fix Violations

If parent is red → violation.

Fix using:

* Recoloring
* Left rotation
* Right rotation

Same red-black balancing logic as `std::map`.

---

## 5️⃣ Rotations (Core Operation)

Example: Left rotation

Before:

```
    A
     \
      B
       \
        C
```

After:

```
      B
     / \
    A   C
```

Rotations maintain BST order but adjust height.

---

## 6️⃣ Searching

Standard BST search:

```cpp
Node* find(const Key& key) {
    Node* curr = root;

    while (curr) {
        if (key == curr->key)
            return curr;
        else if (key < curr->key)
            curr = curr->left;
        else
            curr = curr->right;
    }
    return nullptr;
}
```

Time complexity: O(log n)

---

## 7️⃣ Iterator Implementation

Iterator performs **inorder traversal**.

Why?

Inorder traversal of BST → sorted order.

Example:

```
      8
     / \
    3   10
   / \
  1   6
```

Inorder result:

```
1 3 6 8 10
```

Iterator stores:

```cpp
Node* node;
```

Increment:

* If right subtree exists → go to leftmost node of right subtree
* Else → go upward until coming from left child

---

## 8️⃣ Memory Characteristics

Each element is separately allocated:

```cpp
new Node()
```

Memory per element:

* key
* 3 pointers
* 1 color bit

So:

❌ Higher memory overhead than vector
❌ Poor cache locality

But:

✅ Stable iterators
✅ No reallocation
✅ Ordered iteration

---

## 9️⃣ Iterator Invalidation Rules

| Operation | Invalidates          |
| --------- | -------------------- |
| insert    | No                   |
| erase     | Only erased iterator |
| clear     | All                  |

Very stable container.

---

## 🔟 Why No Random Access?

Because structure is tree, not array.

```cpp
s[5];   // Not allowed
```

To reach 5th element → must traverse.

---

## 1️⃣1️⃣ Difference Between set and map Internally

| Feature        | set              | map                |
| -------------- | ---------------- | ------------------ |
| Stored value   | Key              | pair<const Key, T> |
| Duplicate keys | No               | No                 |
| Tree type      | Red-Black        | Red-Black          |
| Memory         | Slightly smaller | Slightly larger    |

---

## 1️⃣2️⃣ Difference Between set and unordered_set

| Feature         | set            | unordered_set    |
| --------------- | -------------- | ---------------- |
| Data structure  | Red-Black Tree | Hash table       |
| Order           | Sorted         | Unordered        |
| Lookup          | O(log n)       | O(1) average     |
| Memory locality | Poor           | Better (buckets) |

---

## 1️⃣3️⃣ Simplified Educational Implementation

```cpp
template<typename Key>
class SimpleSet {
    struct Node {
        Key key;
        Node* left;
        Node* right;
    };

    Node* root = nullptr;

    Node* insert(Node* node, const Key& key) {
        if (!node)
            return new Node{key, nullptr, nullptr};

        if (key < node->key)
            node->left = insert(node->left, key);
        else if (key > node->key)
            node->right = insert(node->right, key);

        return node;
    }

public:
    void insert(const Key& key) {
        root = insert(root, key);
    }
};
```

Real STL version adds:

* Red-black balancing
* Allocator support
* Iterators
* Transparent comparators
* Node extraction (C++17)
* Exception safety

---

