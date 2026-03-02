# [Happy Number](#happy-number)

## 🟢 Happy Number

---

### 📌 Problem

Write an algorithm to determine if a number `n` is a **happy number**.

A number is happy if:

1. Replace the number by the **sum of the squares of its digits**.
2. Repeat the process until:

   * The number becomes `1` → Happy
   * Or it enters a cycle that does not include `1` → Not Happy

Return `true` if `n` is happy, otherwise `false`.

---

### 📌 Examples

```id="ex1"
Input: n = 19
Output: true
Explanation:
19 → 1² + 9² = 82
82 → 8² + 2² = 68
68 → 6² + 8² = 100
100 → 1² + 0² + 0² = 1
```

```id="ex2"
Input: n = 2
Output: false
```

---

# 🔴 1. Brute Force (Simulate Until Large Limit)

### 💡 Idea

Keep transforming number until:

* It becomes 1 → return true
* Or exceeds some large threshold → assume cycle

⚠ Not reliable for cycle detection.

---

### ⏱ Time Complexity

Unknown / unreliable

❌ Not recommended.

---

# 🟡 2. Better Solution (Using HashSet for Cycle Detection)

### 💡 Key Insight

If number repeats → cycle exists → not happy.

Use set to track visited numbers.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    int getNext(int n) {
        int sum = 0;
        while(n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }
        return sum;
    }

    bool isHappy(int n) {
        unordered_set<int> seen;

        while(n != 1 && !seen.count(n)) {
            seen.insert(n);
            n = getNext(n);
        }

        return n == 1;
    }
};
```

---

### ⏱ Time Complexity

O(log N) per transformation
Total small due to cycle bound

### 🗂 Space Complexity

O(K) (cycle length)

✅ Works well.

---

# 🟢 3. Optimum Solution (Floyd’s Cycle Detection – Two Pointers)

### 💡 Even Better Idea

Instead of using extra space:

Use **slow & fast pointer** (like Linked List cycle detection).

* `slow` moves one step.
* `fast` moves two steps.

If they meet → cycle exists.

If any pointer reaches 1 → happy.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    int getNext(int n) {
        int sum = 0;
        while(n > 0) {
            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }
        return sum;
    }

    bool isHappy(int n) {
        int slow = n;
        int fast = getNext(n);

        while(fast != 1 && slow != fast) {
            slow = getNext(slow);
            fast = getNext(getNext(fast));
        }

        return fast == 1;
    }
};
```

---

### ⏱ Time Complexity

O(log N)

### 🗂 Space Complexity

O(1)

⭐ Most optimal solution.

---

# 🔍 Why Cycle Exists?

For non-happy numbers:

Sequence eventually enters fixed loop:

```id="cycle"
4 → 16 → 37 → 58 → 89 → 145 → 42 → 20 → 4
```

This cycle never reaches 1.

---

# 🔥 Why Two-Pointer Works?

We treat number transformation as:

```
n → f(n) → f(f(n)) → ...
```

This forms a sequence like a linked list.

If there is a cycle:

* Slow and fast pointers must meet.

Same concept as:

* Detect Cycle in Linked List.

---

# 🏆 Final Comparison

| Approach    | Time       | Space | Recommended |
| ----------- | ---------- | ----- | ----------- |
| Brute Guess | Unreliable | -     | ❌           |
| HashSet     | O(log N)   | O(K)  | ✅           |
| Floyd Cycle | O(log N)   | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* Digit manipulation
* Cycle detection
* Floyd’s algorithm application
* Mathematical observation

Related problems:

* Linked List Cycle
* Find Duplicate Number
* Sum of Digits transformations

---
