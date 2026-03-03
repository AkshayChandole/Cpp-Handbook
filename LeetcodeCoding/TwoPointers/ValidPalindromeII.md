# [Valid Palindrome II](#valid-palindrome-ii)

## 🟢 Valid Palindrome II

---

### 📌 Problem

Given a string `s`, return `true` if it can be a palindrome after **deleting at most one character**.

---

### 📌 Examples

```id="ex1"
Input: s = "aba"
Output: true
```

```id="ex2"
Input: s = "abca"
Output: true
Explanation:
Delete 'c' → "aba"
```

```id="ex3"
Input: s = "abc"
Output: false
```

---

# 🔴 1. Brute Force (Try Removing Each Character)

---

### 💡 Idea

For each index:

1. Remove that character.
2. Check if resulting string is palindrome.

Also check original string without removal.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    bool isPalindrome(string s) {
        int l = 0, r = s.size() - 1;
        while(l < r) {
            if(s[l++] != s[r--])
                return false;
        }
        return true;
    }

    bool validPalindrome(string s) {
        if(isPalindrome(s))
            return true;

        for(int i = 0; i < s.size(); i++) {
            string temp = s.substr(0, i) + s.substr(i + 1);
            if(isPalindrome(temp))
                return true;
        }

        return false;
    }
};
```

---

### ⏱ Time Complexity

O(n²)

### 🗂 Space Complexity

O(n)

❌ Too slow for large strings.

---

# 🟡 2. Better Solution (Two Pointers with One Deletion)

---

### 💡 Key Insight

Use two pointers:

* `left` at start
* `right` at end

If mismatch:

* Try skipping either:

  * left character
  * right character

Only one skip allowed.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    bool isPalindromeRange(string& s, int l, int r) {
        while(l < r) {
            if(s[l++] != s[r--])
                return false;
        }
        return true;
    }

    bool validPalindrome(string s) {
        int l = 0, r = s.size() - 1;

        while(l < r) {
            if(s[l] != s[r]) {
                return isPalindromeRange(s, l + 1, r) ||
                       isPalindromeRange(s, l, r - 1);
            }
            l++;
            r--;
        }

        return true;
    }
};
```

---

### ⏱ Time Complexity

O(n)

Worst case: one full additional palindrome check.

### 🗂 Space Complexity

O(1)

⭐ Most optimal solution.

---

# 🟢 3. Why Only One Skip Works

When first mismatch occurs:

```id="case"
s[l] != s[r]
```

Only two possibilities:

* Remove s[l]
* Remove s[r]

If neither works → impossible.

Because:

* Only one deletion allowed.

---

# 🔍 Dry Run

Example:

```id="dry1"
s = "abca"
```

Compare:

```id="dry2"
a == a
b != c
```

Try:

1. Skip b → check "aca" → palindrome ✅
2. Skip c → check "aba" → palindrome ✅

Return true.

---

Example:

```id="dry3"
s = "abc"
```

Compare:

```id="dry4"
a != c
```

Try:

* Remove a → "bc" ❌
* Remove c → "ab" ❌

Return false.

---

# 🎯 Important Edge Cases

| Case               | Result      |
| ------------------ | ----------- |
| Length ≤ 2         | Always true |
| Already palindrome | True        |
| Two mismatches     | False       |

---

# 🏆 Final Comparison

| Approach              | Time  | Space | Recommended |
| --------------------- | ----- | ----- | ----------- |
| Remove Each Char      | O(n²) | O(n)  | ❌           |
| Two Pointer Skip Once | O(n)  | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Two-pointer technique
* Greedy thinking
* Edge case handling
* Palindrome logic

Related problems:

* Valid Palindrome I
* Longest Palindromic Substring
* Palindrome Linked List
* Minimum Deletions to Make Palindrome

---

