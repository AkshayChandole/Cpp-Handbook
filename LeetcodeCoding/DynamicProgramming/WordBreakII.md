# [Word Break II](#word-break-ii)

## 🟢 Word Break II

---

### 📌 Problem

Given a string `s` and a dictionary of strings `wordDict`,
return **all possible sentences** where:

* The sentence is formed by inserting spaces in `s`
* Every word exists in `wordDict`

You may reuse words multiple times.

---

### 📌 Examples

```id="ex1"
Input:
s = "catsanddog"
wordDict = ["cat","cats","and","sand","dog"]

Output:
[
  "cats and dog",
  "cat sand dog"
]
```

```id="ex2"
Input:
s = "pineapplepenapple"
wordDict = ["apple","pen","applepen","pine","pineapple"]

Output:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
```

---

# 🔴 1. Brute Force (Pure Recursion)

### 💡 Idea

* Try every prefix
* If prefix in dictionary
* Recursively solve remaining string
* Combine results

⚠ Extremely expensive due to repeated recomputation.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    vector<string> solve(string s, unordered_set<string>& dict) {
        if(s.empty()) return {""};

        vector<string> result;

        for(int i = 1; i <= s.size(); i++) {
            string prefix = s.substr(0, i);

            if(dict.count(prefix)) {
                vector<string> suffixWays = solve(s.substr(i), dict);

                for(string& way : suffixWays) {
                    result.push_back(prefix + 
                        (way.empty() ? "" : " " + way));
                }
            }
        }
        return result;
    }

    vector<string> wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> dict(wordDict.begin(), wordDict.end());
        return solve(s, dict);
    }
};
```

---

### ⏱ Time Complexity

Exponential (Very slow)

### 🗂 Space Complexity

O(N) recursion stack

❌ Not practical.

---

# 🟡 2. Better Solution (Memoization – Top Down DP)

### 💡 Idea

Store results for each substring to avoid recomputation.

Use:

```id="memo"
unordered_map<string, vector<string>> memo;
```

---

### 💻 Code (C++)

```cpp id="memo1"
class Solution {
public:
    unordered_map<string, vector<string>> memo;

    vector<string> solve(string s, unordered_set<string>& dict) {
        if(memo.count(s)) return memo[s];

        if(s.empty()) return {""};

        vector<string> result;

        for(int i = 1; i <= s.size(); i++) {
            string prefix = s.substr(0, i);

            if(dict.count(prefix)) {
                vector<string> suffixWays =
                    solve(s.substr(i), dict);

                for(string& way : suffixWays) {
                    result.push_back(prefix +
                        (way.empty() ? "" : " " + way));
                }
            }
        }

        return memo[s] = result;
    }

    vector<string> wordBreak(string s,
                             vector<string>& wordDict) {
        unordered_set<string> dict(wordDict.begin(),
                                   wordDict.end());
        return solve(s, dict);
    }
};
```

---

### ⏱ Time Complexity

Still exponential in worst case (due to output size)
But much faster than brute force.

### 🗂 Space Complexity

O(N²) for memo storage

✅ Most common expected solution.

---

# 🟢 3. Optimum Practical Approach (DP + Backtracking)

### 💡 Idea

1. First use **Word Break I DP** to check feasibility.
2. Then backtrack only valid paths.

This prunes many invalid branches.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    vector<string> result;

    void backtrack(string& s, int start,
                   unordered_set<string>& dict,
                   vector<bool>& dp,
                   string current) {

        if(start == s.size()) {
            result.push_back(current);
            return;
        }

        for(int end = start + 1; end <= s.size(); end++) {
            string word = s.substr(start, end - start);

            if(dict.count(word) && dp[end]) {
                string next =
                    current.empty() ? word : current + " " + word;

                backtrack(s, end, dict, dp, next);
            }
        }
    }

    vector<string> wordBreak(string s,
                             vector<string>& wordDict) {

        unordered_set<string> dict(wordDict.begin(),
                                   wordDict.end());
        int n = s.size();

        // DP feasibility check
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

        if(!dp[n]) return {};

        backtrack(s, 0, dict, dp, "");
        return result;
    }
};
```

---

### ⏱ Time Complexity

Worst case still exponential (because we must output all combinations).

But pruning reduces unnecessary recursion.

### 🗂 Space Complexity

O(N²) + output space

⭐ Most practical optimized solution.

---

# 🎯 Important Insight

Unlike Word Break I:

* We must return **all possible sentences**
* So worst-case complexity depends on output size

Example worst case:

```id="worst"
s = "aaaaaaa"
dict = ["a","aa","aaa"]
```

Explodes combinatorially.

---

# 🏆 Final Comparison

| Approach          | Time         | Space | Recommended |
| ----------------- | ------------ | ----- | ----------- |
| Pure Recursion    | Exponential  | O(N)  | ❌           |
| Memoization       | Exponential* | O(N²) | ✅           |
| DP + Backtracking | Exponential* | O(N²) | ⭐ Best      |

(* exponential due to output size)

---

# 🎯 Interview Insight

This is classic:

* DFS + Memo
* Backtracking + DP pruning
* Output-sensitive complexity

Closely related to:

* Palindrome Partitioning
* Combination Sum
* Restore IP Addresses

---

