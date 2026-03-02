# [Reorder List](#Reorder-List)


## 🟢 Reorder List

---

### 📌 Problem

You are given the head of a singly linked list:

```
L0 → L1 → L2 → ... → Ln-1 → Ln
```

Reorder it to:

```
L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...
```

⚠ You must modify the list **in-place**.

---

### 📌 Example

```id="ex1"
Input: 1 → 2 → 3 → 4
Output: 1 → 4 → 2 → 3
```

```id="ex2"
Input: 1 → 2 → 3 → 4 → 5
Output: 1 → 5 → 2 → 4 → 3
```

---

# 🔴 1. Brute Force (Using Extra Array)

### 💡 Idea

1. Store all nodes in a vector.
2. Reconnect nodes from both ends alternately.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    void reorderList(ListNode* head) {
        if(!head) return;

        vector<ListNode*> nodes;
        ListNode* curr = head;

        while(curr) {
            nodes.push_back(curr);
            curr = curr->next;
        }

        int i = 0, j = nodes.size() - 1;

        while(i < j) {
            nodes[i]->next = nodes[j];
            i++;
            if(i >= j) break;

            nodes[j]->next = nodes[i];
            j--;
        }

        nodes[i]->next = nullptr;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(N)

❌ Extra space used.

---

# 🟡 2. Better Solution (Stack Based)

### 💡 Idea

1. Push all nodes into stack.
2. Traverse first half and pop from stack to reorder.

Still uses O(N) space.

---

# 🟢 3. Optimum Solution (Reverse + Merge)

### 💡 Key Insight

Reordering can be broken into 3 steps:

1. Find middle of linked list.
2. Reverse second half.
3. Merge both halves alternately.

---

## 🔹 Step 1: Find Middle (Slow & Fast Pointer)

```cpp
while(fast && fast->next) {
    slow = slow->next;
    fast = fast->next->next;
}
```

---

## 🔹 Step 2: Reverse Second Half

Standard linked list reversal.

---

## 🔹 Step 3: Merge Alternately

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    void reorderList(ListNode* head) {
        if(!head || !head->next) return;

        // Step 1: Find middle
        ListNode* slow = head;
        ListNode* fast = head;

        while(fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // Step 2: Reverse second half
        ListNode* second = slow->next;
        slow->next = nullptr;

        ListNode* prev = nullptr;
        while(second) {
            ListNode* temp = second->next;
            second->next = prev;
            prev = second;
            second = temp;
        }

        // Step 3: Merge two halves
        ListNode* first = head;
        second = prev;

        while(second) {
            ListNode* temp1 = first->next;
            ListNode* temp2 = second->next;

            first->next = second;
            second->next = temp1;

            first = temp1;
            second = temp2;
        }
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

⭐ Most expected interview solution.

---

# 🔍 Dry Run

Input:

```
1 → 2 → 3 → 4 → 5
```

### Step 1: Find Middle

```
First half: 1 → 2 → 3
Second half: 4 → 5
```

### Step 2: Reverse Second Half

```
5 → 4
```

### Step 3: Merge

```
1 → 5 → 2 → 4 → 3
```

---

# 🎯 Why This Works

Instead of jumping back and forth:

* We reverse second half.
* Then merge like zipper:

```
First:  L0 → L1 → L2
Second: Ln → Ln-1 → Ln-2
```

Alternate merge gives required order.

---

# 🏆 Final Comparison

| Approach        | Time | Space | Recommended |
| --------------- | ---- | ----- | ----------- |
| Vector          | O(N) | O(N)  | ❌           |
| Stack           | O(N) | O(N)  | ❌           |
| Reverse + Merge | O(N) | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This is a classic linked list pattern:

* Find middle
* Reverse half
* Merge two lists

Related problems:

* Reverse Linked List
* Merge Two Lists
* Palindrome Linked List
* Split Linked List

---

