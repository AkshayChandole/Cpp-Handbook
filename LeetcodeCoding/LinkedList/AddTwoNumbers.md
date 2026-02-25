# [Add Two Numbers](#add-two-numbers)

## 🧩 Problem: Add Two Numbers

You are given two **non-empty linked lists** representing two non-negative integers.
The digits are stored in **reverse order**, and each node contains a single digit.

Add the two numbers and return the sum as a linked list.

You may assume:

* The two numbers do not contain any leading zero, except the number 0 itself.

---

## 🔎 Examples

**Example 1:**

```id="ex1"
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807
```

**Example 2:**

```id="ex2"
Input: l1 = [0], l2 = [0]
Output: [0]
```

**Example 3:**

```id="ex3"
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

---

# 🐢 Brute Force Solution

### 💡 Idea

1. Convert both linked lists into integers.
2. Add them.
3. Convert result back into a linked list.

⚠️ Problem: Numbers can be very large (overflow issue).

---

### 🔹 Algorithm

1. Traverse `l1` and compute number1.
2. Traverse `l2` and compute number2.
3. Compute `sum = number1 + number2`.
4. Convert sum into reversed linked list.

---

### ⏱ Time Complexity

* **O(max(n, m))**

### 📦 Space Complexity

* **O(max(n, m))**

⚠️ Not safe for very large inputs due to integer overflow.

---

### 💻 C++ Code (Not Recommended)

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        long long n1 = 0, n2 = 0, place = 1;
        
        while(l1) {
            n1 += l1->val * place;
            place *= 10;
            l1 = l1->next;
        }
        
        place = 1;
        while(l2) {
            n2 += l2->val * place;
            place *= 10;
            l2 = l2->next;
        }
        
        long long sum = n1 + n2;
        
        ListNode* dummy = new ListNode(0);
        ListNode* curr = dummy;
        
        if(sum == 0) return new ListNode(0);
        
        while(sum > 0) {
            curr->next = new ListNode(sum % 10);
            sum /= 10;
            curr = curr->next;
        }
        
        return dummy->next;
    }
};
```

---

# 🚀 Better Solution (Using String Addition)

### 💡 Idea

1. Convert linked lists into strings.
2. Reverse them.
3. Perform string-based addition.
4. Build result list.

Avoids integer overflow but still extra work.

---

### ⏱ Time Complexity

* **O(n + m)**

### 📦 Space Complexity

* **O(n + m)**

⚠️ Still not optimal because extra string storage is used.

---

# ⚡ Optimum Solution (Digit-by-Digit Addition)

### 💡 Idea

Simulate manual addition like we do on paper.

* Add digits from both lists.
* Keep track of carry.
* Create new node for each digit.
* Continue until both lists AND carry are exhausted.

---

### 🔹 Algorithm

1. Create dummy node.
2. Initialize `carry = 0`.
3. While `l1` OR `l2` OR `carry`:

   * `sum = carry`
   * If `l1` exists → add value
   * If `l2` exists → add value
   * `carry = sum / 10`
   * Create node with `sum % 10`
4. Return `dummy->next`

---

### ⏱ Time Complexity

* **O(max(n, m))**

### 📦 Space Complexity

* **O(max(n, m))** (for result list)

---

### 💻 C++ Code (Recommended)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */

class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* dummy = new ListNode(0);
        ListNode* curr = dummy;
        int carry = 0;

        while(l1 != NULL || l2 != NULL || carry != 0) {
            int sum = carry;
            
            if(l1 != NULL) {
                sum += l1->val;
                l1 = l1->next;
            }
            
            if(l2 != NULL) {
                sum += l2->val;
                l2 = l2->next;
            }
            
            carry = sum / 10;
            curr->next = new ListNode(sum % 10);
            curr = curr->next;
        }

        return dummy->next;
    }
};
```

---

# 🏆 Final Recommendation

| Approach           | Time     | Space | Recommendation  |
| ------------------ | -------- | ----- | --------------- |
| Convert to Integer | O(n)     | O(n)  | ❌ Overflow risk |
| String Addition    | O(n)     | O(n)  | ⚠️ Extra work   |
| Digit-by-Digit     | **O(n)** | O(n)  | ⭐ Best          |


