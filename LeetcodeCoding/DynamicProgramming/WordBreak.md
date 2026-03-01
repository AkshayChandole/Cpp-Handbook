# [Word Break](#Word-Break)

## 🟢 Word Break

---

### 📌 Problem

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

You may reuse words multiple times.

---

### 📌 Examples

```id="ex1"
Input: s = "leetcode"
wordDict = ["leet","code"]

Output: true
Explanation: "leet code"
```

```id="ex2"
Input: s = "applepenapple"
wordDict = ["apple","pen"]

Output: true
Explanation: "apple pen apple"
```

```id="ex3"
Input: s = "catsandog"
wordDict = ["cats","dog","sand","and","cat"]

Output: false
```

---

# 🔴 1. Brute Force (Pure Recursion)

### 💡 Idea

Try every possible prefix:

* If prefix is in dictionary
* Recursively check remaining string

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    bool solve(string s, unordered_set<string>& dict) {
        if(s.empty()) return true;

        for(int i = 1; i <= s.size(); i++) {
            string prefix = s.substr(0, i);

            if(dict.count(prefix) &&
               solve(s.substr(i), dict))
                return true;
        }
        return false;
    }

    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> dict(wordDict.begin(), wordDict.end());
        return solve(s, dict);
    }
};
```

---

### ⏱ Time Complexity

O(2^N) (exponential)

### 🗂 Space Complexity

O(N) recursion stack

❌ Too slow (repeated computations).

---

# 🟡 2. Better Solution (Memoization – Top Down DP)

### 💡 Idea

Avoid recomputation by storing results of substrings.

Use `dp[startIndex]`:

* True → substring from startIndex can be segmented
* False → cannot

---

### 💻 Code (C++)

```cpp id="memo1"
class Solution {
public:
    bool solve(int start, string& s,
               unordered_set<string>& dict,
               vector<int>& dp) {

        if(start == s.size()) return true;
        if(dp[start] != -1) return dp[start];

        for(int end = start + 1; end <= s.size(); end++) {
            string word = s.substr(start, end - start);

            if(dict.count(word) &&
               solve(end, s, dict, dp))
                return dp[start] = true;
        }

        return dp[start] = false;
    }

    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> dict(wordDict.begin(), wordDict.end());
        vector<int> dp(s.size(), -1);

        return solve(0, s, dict, dp);
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(N)

✅ Much better.

---

# 🟢 3. Optimum Solution (Bottom-Up DP)

### 💡 Idea

Let:

```id="form"
dp[i] = true
```

If substring `s[0..i-1]` can be segmented.

We build solution iteratively.

---

### 🔹 Algorithm

1. `dp[0] = true` (empty string)
2. For each `i` from 1 to n:

   * For each `j` from 0 to i:

     * If `dp[j] == true`
     * and `s[j:i]` is in dictionary
       → set `dp[i] = true`

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> dict(wordDict.begin(), wordDict.end());
        int n = s.size();

        vector<bool> dp(n + 1, false);
        dp[0] = true;

        for(int i = 1; i <= n; i++) {
            for(int j = 0; j < i; j++) {
                if(dp[j] &&
                   dict.count(s.substr(j, i - j))) {
                    dp[i] = true;
                    break;
                }
            }
        }

        return dp[n];
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(N)

⭐ Most expected interview answer.

---

# 🔵 4. Further Optimization (Limit Word Length)

If we compute:

```id="len"
maxWordLength
```

We can limit inner loop to:

```id="loop"
j >= i - maxWordLength
```

This improves performance significantly.

---

# 🎯 Why This Is DP

At index `i`:
We check if there exists a valid break at some `j < i`.

So:

[
dp[i] = \exists j < i \text{ such that } dp[j] = true \land s[j:i] \in dict
]

---

# 🏆 Final Comparison

| Approach     | Time   | Space | Recommended |
| ------------ | ------ | ----- | ----------- |
| Recursion    | O(2^N) | O(N)  | ❌           |
| Memoization  | O(N²)  | O(N)  | ✅           |
| Bottom-Up DP | O(N²)  | O(N)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* Recursion → Memoization → DP progression
* String segmentation DP pattern
* Similar to:

  * Palindrome Partitioning
  * Decode Ways
  * Coin Change (boolean form)

---

