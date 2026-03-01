# [Merge Two Sorted Lists](#merge-two-sorted-lists)

## 🟢 Merge Two Sorted Lists

---

### 📌 Problem

You are given the heads of two **sorted linked lists** `list1` and `list2`.

Merge the two lists into one **sorted linked list** and return its head.

The merged list should be made by splicing together the nodes of the first two lists.

---

### 📌 Example

```id="ex1"
Input:
list1 = 1 -> 2 -> 4
list2 = 1 -> 3 -> 4

Output:
1 -> 1 -> 2 -> 3 -> 4 -> 4
```

```id="ex2"
Input:
list1 = []
list2 = []

Output:
[]
```

---

# 🔴 1. Brute Force (Using Extra Array)

### 💡 Idea

1. Traverse both lists
2. Store elements in a vector
3. Sort the vector
4. Create new linked list

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        vector<int> vals;

        while(list1) {
            vals.push_back(list1->val);
            list1 = list1->next;
        }

        while(list2) {
            vals.push_back(list2->val);
            list2 = list2->next;
        }

        sort(vals.begin(), vals.end());

        ListNode dummy(0);
        ListNode* tail = &dummy;

        for(int v : vals) {
            tail->next = new ListNode(v);
            tail = tail->next;
        }

        return dummy.next;
    }
};
```

---

### ⏱ Time Complexity

O((N + M) log(N + M))

### 🗂 Space Complexity

O(N + M)

❌ Not optimal because sorting is unnecessary.

---

# 🟡 2. Better Solution (Iterative Merge)

### 💡 Idea

Use two pointers:

* Compare nodes
* Attach smaller node
* Move pointer
* Continue until one list ends
* Attach remaining nodes

---

### 💻 Code (C++)

```cpp id="iter1"
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode dummy(0);
        ListNode* tail = &dummy;

        while(list1 && list2) {
            if(list1->val <= list2->val) {
                tail->next = list1;
                list1 = list1->next;
            } else {
                tail->next = list2;
                list2 = list2->next;
            }
            tail = tail->next;
        }

        if(list1) tail->next = list1;
        if(list2) tail->next = list2;

        return dummy.next;
    }
};
```

---

### ⏱ Time Complexity

O(N + M)

### 🗂 Space Complexity

O(1)

✅ Most common interview solution.

---

# 🟢 3. Optimum Solution (Recursive Merge)

### 💡 Idea

Recursive thinking:

* If one list is empty → return other
* Choose smaller node
* Recursively merge remaining lists

---

### 💻 Code (C++)

```cpp id="rec1"
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        if(!list1) return list2;
        if(!list2) return list1;

        if(list1->val <= list2->val) {
            list1->next = mergeTwoLists(list1->next, list2);
            return list1;
        } else {
            list2->next = mergeTwoLists(list1, list2->next);
            return list2;
        }
    }
};
```

---

### ⏱ Time Complexity

O(N + M)

### 🗂 Space Complexity

O(N + M) recursion stack

---

# 🎯 Why Iterative is Preferred

* Same time complexity
* Uses O(1) extra space
* Avoids recursion stack overflow
* Clean and easy to understand

---

# 🏆 Final Comparison

| Approach           | Time              | Space  | Recommended |
| ------------------ | ----------------- | ------ | ----------- |
| Extra Array + Sort | O((N+M) log(N+M)) | O(N+M) | ❌           |
| Iterative Merge    | O(N+M)            | O(1)   | ⭐ Best      |
| Recursive          | O(N+M)            | O(N+M) | ✅           |

---
