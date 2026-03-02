# [Sort List](#Sort-List)

## 🟢 Sort List

---

### 📌 Problem

Given the head of a linked list, return the list after sorting it in ascending order.

Constraints:

* Time complexity must be **O(N log N)**
* Space complexity should be **O(1)** (constant space)

---

### 📌 Example

```id="ex1"
Input: head = [4,2,1,3]
Output: [1,2,3,4]
```

```id="ex2"
Input: head = [-1,5,3,4,0]
Output: [-1,0,3,4,5]
```

---

# 🔴 1. Brute Force (Copy to Array + Sort)

### 💡 Idea

1. Copy all values into vector.
2. Sort vector.
3. Rewrite linked list.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if(!head) return head;

        vector<int> vals;
        ListNode* curr = head;

        while(curr) {
            vals.push_back(curr->val);
            curr = curr->next;
        }

        sort(vals.begin(), vals.end());

        curr = head;
        for(int val : vals) {
            curr->val = val;
            curr = curr->next;
        }

        return head;
    }
};
```

---

### ⏱ Time Complexity

O(N log N)

### 🗂 Space Complexity

O(N)

❌ Not constant space.

---

# 🟡 2. Better Solution (Insertion Sort on List)

### 💡 Idea

Insert each node into correct position in a sorted list.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if(!head) return head;

        ListNode dummy(0);

        while(head) {
            ListNode* prev = &dummy;

            while(prev->next && prev->next->val < head->val)
                prev = prev->next;

            ListNode* next = head->next;
            head->next = prev->next;
            prev->next = head;
            head = next;
        }

        return dummy.next;
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(1)

❌ Too slow for large lists.

---

# 🟢 3. Optimum Solution (Merge Sort on Linked List)

### 💡 Why Merge Sort?

* Linked lists do not allow random access → QuickSort not ideal.
* Merge sort:

  * Naturally works with linked lists.
  * Requires no extra arrays.
  * Achieves O(N log N).

---

## 🔹 Step 1: Find Middle

Use slow and fast pointer.

---

## 🔹 Step 2: Split List

Break into two halves.

---

## 🔹 Step 3: Recursively Sort Both Halves

---

## 🔹 Step 4: Merge Two Sorted Lists

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    ListNode* merge(ListNode* l1, ListNode* l2) {
        ListNode dummy(0);
        ListNode* tail = &dummy;

        while(l1 && l2) {
            if(l1->val < l2->val) {
                tail->next = l1;
                l1 = l1->next;
            } else {
                tail->next = l2;
                l2 = l2->next;
            }
            tail = tail->next;
        }

        tail->next = l1 ? l1 : l2;
        return dummy.next;
    }

    ListNode* sortList(ListNode* head) {
        if(!head || !head->next)
            return head;

        // Find middle
        ListNode* slow = head;
        ListNode* fast = head->next;

        while(fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        ListNode* mid = slow->next;
        slow->next = nullptr;

        ListNode* left = sortList(head);
        ListNode* right = sortList(mid);

        return merge(left, right);
    }
};
```

---

### ⏱ Time Complexity

O(N log N)

### 🗂 Space Complexity

O(log N) (recursion stack)

⭐ Most expected interview solution.

---

# 🔍 Why Merge Sort Works Best Here

* Splitting list is O(N)
* Merging is O(N)
* Depth of recursion = log N

Total:

[
O(N \log N)
]

---

# 🔥 Important Detail

Notice:

```cpp
ListNode* fast = head->next;
```

This ensures proper splitting for even-length lists.

---

# 🏆 Final Comparison

| Approach       | Time       | Space    | Recommended |
| -------------- | ---------- | -------- | ----------- |
| Copy + Sort    | O(N log N) | O(N)     | ❌           |
| Insertion Sort | O(N²)      | O(1)     | ❌           |
| Merge Sort     | O(N log N) | O(log N) | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* Merge sort on linked list
* Splitting using slow/fast pointer
* Recursion divide & conquer

Related problems:

* Merge Two Sorted Lists
* Reorder List
* Reverse Linked List
* K Sorted Lists Merge

---

