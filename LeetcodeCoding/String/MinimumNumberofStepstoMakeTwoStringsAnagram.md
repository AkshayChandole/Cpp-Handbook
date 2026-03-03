# [Minimum Number of Steps to Make Two Strings Anagram](#minimum-number-of-steps-to-make-two-strings-anagram)

## 🟢 Minimum Number of Steps to Make Two Strings Anagram

---

### 📌 Problem

You are given two strings `s` and `t` of the same length.

In one step, you can choose any character in `t` and replace it with another character.

Return the **minimum number of steps** required to make `t` an anagram of `s`.

---

### 📌 Examples

```id="ex1"
Input: s = "bab", t = "aba"
Output: 1
Explanation:
Replace one 'a' in t with 'b'.
```

```id="ex2"
Input: s = "leetcode", t = "practice"
Output: 5
```

```id="ex3"
Input: s = "anagram", t = "mangaar"
Output: 0
```

---

# 🔴 1. Brute Force (Sort and Compare)

### 💡 Idea

1. Sort both strings.
2. Compare character by character.
3. Count mismatches.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int minSteps(string s, string t) {
        sort(s.begin(), s.end());
        sort(t.begin(), t.end());

        int steps = 0;

        for(int i = 0; i < s.size(); i++) {
            if(s[i] != t[i])
                steps++;
        }

        return steps;
    }
};
```

---

### ⏱ Time Complexity

O(N log N)

### 🗂 Space Complexity

O(1)

❌ Incorrect logic in some cases (overcounts).

---

# 🟡 2. Better Solution (Frequency Counting)

### 💡 Key Insight

We only need to make `t` match `s`.

Count frequencies:

* freqS
* freqT

If `t` has fewer occurrences of a character than `s`,
we need to replace characters in `t`.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    int minSteps(string s, string t) {
        vector<int> freq(26, 0);

        for(char c : s)
            freq[c - 'a']++;

        for(char c : t)
            freq[c - 'a']--;

        int steps = 0;

        for(int f : freq) {
            if(f > 0)
                steps += f;
        }

        return steps;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

✅ Efficient and clean.

---

# 🟢 3. Optimum Explanation (Mathematical View)

### 💡 Why Summing Positive Differences Works

Let:

```id="freq"
freq[c] = countS[c] - countT[c]
```

If:

```id="positive"
freq[c] > 0
```

It means:

* `t` is missing `freq[c]` copies of character `c`
* We must replace that many characters in `t`

We ignore negative values because:

* Extra characters in `t` will be replaced automatically.

---

# 🔍 Dry Run

Example:

```id="dry1"
s = "leetcode"
t = "practice"
```

Compute difference:

| Character | s | t | diff |
| --------- | - | - | ---- |
| l         | 1 | 0 | +1   |
| e         | 3 | 1 | +2   |
| t         | 1 | 1 | 0    |
| c         | 1 | 2 | -1   |
| p         | 0 | 1 | -1   |
| r         | 0 | 1 | -1   |
| a         | 0 | 1 | -1   |
| i         | 0 | 1 | -1   |

Sum of positive diffs:

```id="result"
1 + 2 + 1 + 1 = 5
```

Answer = 5

---

# 🎯 Why Only Count Positive?

Because:

Total missing characters in `t` equals total extra characters.

So counting positive differences is sufficient.

---

# 🏆 Final Comparison

| Approach       | Time       | Space | Recommended |
| -------------- | ---------- | ----- | ----------- |
| Sort & Compare | O(N log N) | O(1)  | ❌           |
| Frequency Diff | O(N)       | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* Frequency array technique
* Difference counting
* Anagram transformation logic

Related problems:

* Valid Anagram
* Ransom Note
* Find All Anagrams in a String
* Minimum Deletions to Make Strings Equal

---

