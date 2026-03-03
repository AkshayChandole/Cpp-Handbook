# [Add Two Numbers II](#add-two-numbers-ii)

## 🟢 Add Two Numbers II

---

### 📌 Problem

You are given two **non-empty linked lists** representing two non-negative integers.

* Digits are stored in **forward order**
* Each node contains a single digit
* Add the two numbers and return the sum as a linked list

You **cannot reverse** the input lists (follow-up constraint).

---

### 📌 Example

```id="ex1"
Input:
l1 = [7,2,4,3]
l2 = [5,6,4]

Output:
[7,8,0,7]

Explanation:
7243 + 564 = 7807
```

```id="ex2"
Input:
l1 = [2,4,3]
l2 = [5,6,4]

Output:
[8,0,7]
```

---

# 🔴 1. Brute Force (Convert to Integer)

### 💡 Idea

1. Convert both linked lists into integers.
2. Add them.
3. Convert result back into linked list.

---

### ❌ Why It Fails

* Numbers can be very large.
* Integer overflow.
* Not allowed in interview.

---

### ⏱ Time Complexity

O(N + M)

### 🗂 Space Complexity

O(N + M)

❌ Not safe.

---

# 🟡 2. Better Solution (Reverse Both Lists)

### 💡 Idea

Since addition is easier from right to left:

1. Reverse both lists.
2. Perform normal addition (like Add Two Numbers I).
3. Reverse result.

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    ListNode* reverse(ListNode* head) {
        ListNode* prev = nullptr;
        while(head) {
            ListNode* next = head->next;
            head->next = prev;
            prev = head;
            head = next;
        }
        return prev;
    }

    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        l1 = reverse(l1);
        l2 = reverse(l2);

        ListNode dummy(0);
        ListNode* curr = &dummy;
        int carry = 0;

        while(l1 || l2 || carry) {
            int sum = carry;

            if(l1) {
                sum += l1->val;
                l1 = l1->next;
            }
            if(l2) {
                sum += l2->val;
                l2 = l2->next;
            }

            carry = sum / 10;
            curr->next = new ListNode(sum % 10);
            curr = curr->next;
        }

        return reverse(dummy.next);
    }
};
```

---

### ⏱ Time Complexity

O(N + M)

### 🗂 Space Complexity

O(1) extra (ignoring output)

✅ Works but modifies input.

---

# 🟢 3. Optimum Solution (Using Stacks)

### 💡 Key Insight

We cannot reverse lists.

Use stacks to simulate reverse traversal.

Steps:

1. Push digits of l1 into stack1.
2. Push digits of l2 into stack2.
3. Pop and add with carry.
4. Insert nodes at front of result.

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        stack<int> s1, s2;

        while(l1) {
            s1.push(l1->val);
            l1 = l1->next;
        }

        while(l2) {
            s2.push(l2->val);
            l2 = l2->next;
        }

        int carry = 0;
        ListNode* head = nullptr;

        while(!s1.empty() || !s2.empty() || carry) {
            int sum = carry;

            if(!s1.empty()) {
                sum += s1.top();
                s1.pop();
            }

            if(!s2.empty()) {
                sum += s2.top();
                s2.pop();
            }

            carry = sum / 10;

            ListNode* node = new ListNode(sum % 10);
            node->next = head;
            head = node;
        }

        return head;
    }
};
```

---

### ⏱ Time Complexity

O(N + M)

### 🗂 Space Complexity

O(N + M) for stacks

⭐ Most expected solution.

---

# 🔍 Dry Run

Example:

```id="dry1"
l1 = [7,2,4,3]
l2 = [5,6,4]
```

Stacks:

```id="dry2"
s1 = 7 2 4 3
s2 = 5 6 4
```

Add:

| Pop | Sum | Node |
| --- | --- | ---- |
| 3+4 | 7   | 7    |
| 4+6 | 10  | 0    |
| 2+5 | 8   | 8    |
| 7   | 7   | 7    |

Result:

```id="dry3"
[7,8,0,7]
```

---

# 🎯 Why Insert at Front?

Because:

* We compute digits from least significant to most significant.
* So we build result from back to front.

---

# 🏆 Final Comparison

| Approach           | Time | Space | Recommended |
| ------------------ | ---- | ----- | ----------- |
| Convert to Integer | O(N) | Risky | ❌           |
| Reverse Lists      | O(N) | O(1)  | ✅           |
| Stack Method       | O(N) | O(N)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Linked list manipulation
* Stack usage
* Carry handling
* Forward vs reverse order logic

Related problems:

* Add Two Numbers I
* Reverse Linked List
* Add Binary
* Multiply Strings

---
