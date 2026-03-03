# [Remove Letter To Equalize Frequency](#remove-letter-to-equalize-frequency)

## 🟢 Remove Letter To Equalize Frequency

---

### 📌 Problem

You are given a string `word`.

Return `true` if it is possible to remove **exactly one character** from `word` so that:

* The frequency of every remaining letter is equal.

Otherwise return `false`.

---

### 📌 Examples

```id="ex1"
Input: word = "abcc"
Output: true
Explanation:
Remove one 'c' → "abc"
Each character appears once.
```

```id="ex2"
Input: word = "aazz"
Output: false
```

```id="ex3"
Input: word = "abc"
Output: true
Explanation:
Remove any character → remaining two both have frequency 1.
```

---

# 🔴 1. Brute Force (Try Removing Each Character)

### 💡 Idea

For each index:

1. Remove that character.
2. Count frequencies.
3. Check if all non-zero frequencies are equal.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    bool equalFrequency(string word) {
        for(int i = 0; i < word.size(); i++) {
            vector<int> freq(26, 0);

            for(int j = 0; j < word.size(); j++) {
                if(i == j) continue;
                freq[word[j] - 'a']++;
            }

            int target = 0;
            bool valid = true;

            for(int f : freq) {
                if(f > 0) {
                    if(target == 0)
                        target = f;
                    else if(f != target) {
                        valid = false;
                        break;
                    }
                }
            }

            if(valid)
                return true;
        }

        return false;
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(1)

❌ Inefficient for larger strings.

---

# 🟡 2. Better Solution (Frequency Map + Try Reducing One)

### 💡 Idea

1. Count frequency of each character.
2. For each character:

   * Reduce its frequency by 1.
   * Check if remaining frequencies are equal.
   * Restore frequency.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    bool equalFrequency(string word) {
        vector<int> freq(26, 0);

        for(char c : word)
            freq[c - 'a']++;

        for(int i = 0; i < 26; i++) {
            if(freq[i] == 0) continue;

            freq[i]--;

            int target = 0;
            bool valid = true;

            for(int f : freq) {
                if(f > 0) {
                    if(target == 0)
                        target = f;
                    else if(f != target) {
                        valid = false;
                        break;
                    }
                }
            }

            freq[i]++;

            if(valid)
                return true;
        }

        return false;
    }
};
```

---

### ⏱ Time Complexity

O(26 × 26) ≈ O(1)

Since alphabet size fixed.

### 🗂 Space Complexity

O(1)

✅ Much better.

---

# 🟢 3. Optimum Mathematical Observation

### 💡 Key Insight

Let’s analyze frequency counts.

Let:

```id="freq"
charFreq → frequency of characters
countFreq → frequency of frequencies
```

Valid cases:

### Case 1️⃣

All characters already have same frequency
AND one character has frequency 1
→ Remove that character completely

Example:

```id="case1"
aabbccddx
freq: 2,2,2,2,1
```

Remove `x` → all 2.

---

### Case 2️⃣

All characters have same frequency except one character
whose frequency is exactly one more.

Example:

```id="case2"
aabbccc
freq: 2,2,3
```

Remove one `c` → 2,2,2

---

### 💻 Code (C++ – Cleanest)

```cpp id="opt1"
class Solution {
public:
    bool equalFrequency(string word) {
        unordered_map<char, int> freq;

        for(char c : word)
            freq[c]++;

        unordered_map<int, int> countFreq;

        for(auto& [c, f] : freq)
            countFreq[f]++;

        if(countFreq.size() == 1) {
            auto [f, count] = *countFreq.begin();
            return f == 1 || count == 1;
        }

        if(countFreq.size() == 2) {
            auto it = countFreq.begin();
            int f1 = it->first, c1 = it->second;
            it++;
            int f2 = it->first, c2 = it->second;

            // Ensure f1 < f2
            if(f1 > f2) {
                swap(f1, f2);
                swap(c1, c2);
            }

            // Case 1: one char has freq 1
            if(f1 == 1 && c1 == 1)
                return true;

            // Case 2: one char has freq one more
            if(f2 == f1 + 1 && c2 == 1)
                return true;
        }

        return false;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

⭐ Most optimal and elegant solution.

---

# 🔍 Why Only These Cases Work?

Because removing one character can:

* Remove a full frequency group (if frequency = 1)
* Reduce one frequency by 1

No other transformation possible.

---

# 🏆 Final Comparison

| Approach          | Time  | Space | Recommended |
| ----------------- | ----- | ----- | ----------- |
| Try Remove Each   | O(N²) | O(1)  | ❌           |
| Reduce Each Freq  | O(1)  | O(1)  | ✅           |
| Frequency-of-Freq | O(N)  | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Frequency counting
* Edge case analysis
* Logical case breakdown
* HashMap usage

Related problems:

* Valid Anagram
* Ransom Note
* Group Anagrams
* Minimum Deletions to Make Frequencies Unique

---

