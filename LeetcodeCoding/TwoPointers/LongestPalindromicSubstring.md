# [Longest Palindromic Substring](#longest-palindromic-substring)

## 🧩 Problem: Longest Palindromic Substring

Given a string `s`, return the **longest palindromic substring** in `s`.

A palindrome is a string that reads the same forward and backward.

---

## 🔎 Examples

**Example 1:**

```id="ex1"
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

**Example 2:**

```id="ex2"
Input: s = "cbbd"
Output: "bb"
```

**Example 3:**

```id="ex3"
Input: s = "a"
Output: "a"
```

---

# 🐢 Brute Force Solution

### 💡 Idea

Generate all substrings and check each one if it is a palindrome.

---

### 🔹 Algorithm

1. Generate all substrings using two loops.
2. For each substring:

   * Check if it is palindrome.
3. Track longest palindrome.

---

### ⏱ Time Complexity

* Substrings: **O(n²)**
* Palindrome check: **O(n)**
* Total: **O(n³)**

### 📦 Space Complexity

* **O(1)**

---

### 💻 C++ Code

```cpp
class Solution {
public:
    bool isPalindrome(string &s, int left, int right) {
        while(left < right) {
            if(s[left] != s[right]) return false;
            left++;
            right--;
        }
        return true;
    }

    string longestPalindrome(string s) {
        int n = s.size();
        string ans = "";

        for(int i = 0; i < n; i++) {
            for(int j = i; j < n; j++) {
                if(isPalindrome(s, i, j)) {
                    if(j - i + 1 > ans.size()) {
                        ans = s.substr(i, j - i + 1);
                    }
                }
            }
        }
        return ans;
    }
};
```

---

# 🚀 Better Solution (Dynamic Programming)

### 💡 Idea

Use DP table where:

```
dp[i][j] = true  if substring from i to j is palindrome
```

Conditions:

* Single character → palindrome
* Two characters → palindrome if equal
* More than 2 characters →
  `s[i] == s[j] && dp[i+1][j-1]`

---

### 🔹 Algorithm

1. Initialize `dp[n][n]` to false.
2. Fill table diagonally.
3. Track longest palindrome.

---

### ⏱ Time Complexity

* **O(n²)**

### 📦 Space Complexity

* **O(n²)**

---

### 💻 C++ Code

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        vector<vector<bool>> dp(n, vector<bool>(n, false));
        
        int start = 0, maxLen = 1;

        for(int i = 0; i < n; i++)
            dp[i][i] = true;

        for(int len = 2; len <= n; len++) {
            for(int i = 0; i <= n - len; i++) {
                int j = i + len - 1;

                if(s[i] == s[j]) {
                    if(len == 2 || dp[i+1][j-1]) {
                        dp[i][j] = true;

                        if(len > maxLen) {
                            start = i;
                            maxLen = len;
                        }
                    }
                }
            }
        }

        return s.substr(start, maxLen);
    }
};
```

---

# ⚡ Optimum Solution (Expand Around Center)

### 💡 Idea

A palindrome expands around its center.

For each index:

* Expand for **odd length** palindrome
* Expand for **even length** palindrome

Total centers = `2n - 1`

---

### 🔹 Algorithm

1. For each index `i`:

   * Expand with `(i, i)` → odd length
   * Expand with `(i, i+1)` → even length
2. Keep updating longest palindrome.

---

### ⏱ Time Complexity

* **O(n²)**

### 📦 Space Complexity

* **O(1)**

---

### 💻 C++ Code (Recommended)

```cpp
class Solution {
public:
    string expand(string &s, int left, int right) {
        while(left >= 0 && right < s.size() && s[left] == s[right]) {
            left--;
            right++;
        }
        return s.substr(left + 1, right - left - 1);
    }

    string longestPalindrome(string s) {
        string result = "";

        for(int i = 0; i < s.size(); i++) {
            string odd = expand(s, i, i);
            if(odd.size() > result.size())
                result = odd;

            string even = expand(s, i, i + 1);
            if(even.size() > result.size())
                result = even;
        }

        return result;
    }
};
```

---

# 🏆 Final Recommendation

| Approach             | Time      | Space    | Recommendation    |
| -------------------- | --------- | -------- | ----------------- |
| Brute Force          | O(n³)     | O(1)     | ❌ Too slow        |
| DP                   | O(n²)     | O(n²)    | ⚠️ Good but heavy |
| Expand Around Center | **O(n²)** | **O(1)** | ⭐ Best            |

👉 In interviews, use **Expand Around Center** — clean, optimal space, and easy to implement.

---

