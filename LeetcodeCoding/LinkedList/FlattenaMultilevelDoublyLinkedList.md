# [Flatten a Multilevel Doubly Linked List](#flatten-a-multilevel-doubly-linked-list)

## 🟢 Flatten a Multilevel Doubly Linked List

---

### 📌 Problem

You are given a doubly linked list where in addition to `next` and `prev` pointers, a node may also have a `child` pointer pointing to a separate doubly linked list.

These child lists may have one or more children of their own.

Return the list after **flattening it into a single-level doubly linked list**.

---

### 📌 Example

```
1---2---3---4---5---6
        |
        7---8---9---10
            |
            11--12
```

After flattening:

```
1-2-3-7-8-11-12-9-10-4-5-6
```

---

# 🔴 1. Brute Force (Using Extra Vector)

### 💡 Idea

* Perform DFS traversal
* Store nodes in a vector
* Reconnect them in order

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    void dfs(Node* head, vector<Node*>& nodes) {
        while(head) {
            nodes.push_back(head);

            if(head->child) {
                dfs(head->child, nodes);
            }

            head = head->next;
        }
    }

    Node* flatten(Node* head) {
        if(!head) return head;

        vector<Node*> nodes;
        dfs(head, nodes);

        for(int i = 0; i < nodes.size(); i++) {
            nodes[i]->child = nullptr;
            nodes[i]->prev = (i == 0 ? nullptr : nodes[i-1]);
            nodes[i]->next = (i == nodes.size()-1 ? nullptr : nodes[i+1]);
        }

        return nodes[0];
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(N)

❌ Uses extra memory.

---

# 🟡 2. Better Solution (Recursive DFS)

### 💡 Idea

Flatten child list recursively.

Steps:

1. If node has child:

   * Recursively flatten child
   * Insert child between node and node->next
2. Connect tail of child to original next

Return tail of flattened list.

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    Node* dfs(Node* head) {
        Node* curr = head;
        Node* last = nullptr;

        while(curr) {
            Node* next = curr->next;

            if(curr->child) {
                Node* childTail = dfs(curr->child);

                curr->next = curr->child;
                curr->child->prev = curr;
                curr->child = nullptr;

                if(next) {
                    childTail->next = next;
                    next->prev = childTail;
                }

                last = childTail;
            } else {
                last = curr;
            }

            curr = next;
        }

        return last;
    }

    Node* flatten(Node* head) {
        if(!head) return head;
        dfs(head);
        return head;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(N) recursion stack (worst case)

---

# 🟢 3. Optimum Solution (Iterative Using Stack)

### 💡 Idea

Use stack to simulate DFS:

1. Traverse normally
2. If node has child:

   * If node->next exists → push to stack
   * Connect child as next
   * Remove child pointer
3. When reaching end and stack not empty:

   * Pop and connect

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    Node* flatten(Node* head) {
        if(!head) return head;

        stack<Node*> st;
        Node* curr = head;

        while(curr) {
            if(curr->child) {
                if(curr->next)
                    st.push(curr->next);

                curr->next = curr->child;
                curr->child->prev = curr;
                curr->child = nullptr;
            }

            if(!curr->next && !st.empty()) {
                Node* temp = st.top();
                st.pop();

                curr->next = temp;
                temp->prev = curr;
            }

            curr = curr->next;
        }

        return head;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(N) (stack worst case)

---

# 🎯 Why This Works

* Depth-first traversal
* Maintain original order
* Properly reconnect `prev` pointers
* Clear child pointers

---

# 🏆 Final Comparison

| Approach        | Time | Extra Space | Recommended |
| --------------- | ---- | ----------- | ----------- |
| Vector DFS      | O(N) | O(N)        | ❌           |
| Recursive DFS   | O(N) | O(N)        | ✅           |
| Iterative Stack | O(N) | O(N)        | ⭐ Best      |

---

