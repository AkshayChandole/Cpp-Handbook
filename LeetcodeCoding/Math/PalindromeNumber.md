# [Palindrome Number](#palindrome-number)

## 🧩 Problem: Palindrome Number

Given an integer `x`, return **true** if `x` is a palindrome, and **false** otherwise.

A number is a palindrome if it reads the same backward as forward.

⚠️ Negative numbers are **not** palindromes (because of the `-` sign).

---

## 🔎 Examples

**Example 1:**

```id="ex1"
Input: x = 121
Output: true
```

**Example 2:**

```id="ex2"
Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-.
```

**Example 3:**

```id="ex3"
Input: x = 10
Output: false
Explanation: Reads 01 from right to left.
```

---

# 🐢 Brute Force Solution (Convert to String)

### 💡 Idea

1. Convert integer to string.
2. Check if string is palindrome using two pointers.

---

### 🔹 Algorithm

1. Convert `x` to string `s`.
2. Initialize `left = 0`, `right = s.size()-1`.
3. While `left < right`:

   * If `s[left] != s[right]` → return false
4. Return true.

---

### ⏱ Time Complexity

* **O(d)** (d = number of digits)

### 📦 Space Complexity

* **O(d)** (string storage)

---

### 💻 C++ Code

```cpp id="bf1">
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0) return false;

        string s = to_string(x);
        int left = 0, right = s.size() - 1;

        while(left < right) {
            if(s[left] != s[right])
                return false;
            left++;
            right--;
        }
        return true;
    }
};
```

---

# 🚀 Better Solution (Reverse Entire Number)

### 💡 Idea

Reverse the integer mathematically and compare with original.

⚠️ Be careful about overflow.

---

### 🔹 Algorithm

1. Store original number.
2. Reverse digits using `% 10`.
3. Compare reversed number with original.

---

### ⏱ Time Complexity

* **O(d)**

### 📦 Space Complexity

* **O(1)**

---

### 💻 C++ Code

```cpp id="bf2">
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0) return false;

        long long reversed = 0;
        int original = x;

        while(x != 0) {
            reversed = reversed * 10 + x % 10;
            x /= 10;
        }

        return reversed == original;
    }
};
```

---

# ⚡ Optimum Solution (Reverse Half of the Number)

### 💡 Key Observation

We don't need to reverse the entire number.

We can:

* Reverse only **half** of the digits.
* Compare first half with reversed second half.

This avoids overflow.

---

### 🔹 Important Edge Cases

* Negative numbers → false
* Numbers ending in 0 (but not 0 itself) → false
  (Example: 10 → cannot be palindrome)

---

### 🔹 Algorithm

1. If `x < 0` OR (`x % 10 == 0 && x != 0`) → return false
2. Initialize `reversedHalf = 0`
3. While `x > reversedHalf`:

   * `reversedHalf = reversedHalf * 10 + x % 10`
   * `x /= 10`
4. For even digits:

   * `x == reversedHalf`
5. For odd digits:

   * `x == reversedHalf / 10`

---

### ⏱ Time Complexity

* **O(d/2) ≈ O(d)**

### 📦 Space Complexity

* **O(1)**

---

### 💻 C++ Code (Recommended)

```cpp id="opt1">
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0 || (x % 10 == 0 && x != 0))
            return false;

        int reversedHalf = 0;

        while(x > reversedHalf) {
            reversedHalf = reversedHalf * 10 + x % 10;
            x /= 10;
        }

        return (x == reversedHalf || x == reversedHalf / 10);
    }
};
```

---

# 🏆 Final Recommendation

| Approach              | Time     | Space    | Recommendation |
| --------------------- | -------- | -------- | -------------- |
| Convert to String     | O(d)     | O(d)     | ⚠️ Simple      |
| Reverse Entire Number | O(d)     | O(1)     | Good           |
| Reverse Half          | **O(d)** | **O(1)** | ⭐ Best         |

👉 In interviews, use **reverse half technique** — optimal, safe from overflow, and elegant.
