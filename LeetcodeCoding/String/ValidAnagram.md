# [Valid Anagram](#Valid-Anagram)

## 🟢 Valid Anagram

---

### 📌 Problem

Given two strings `s` and `t`, return `true` if `t` is an **anagram** of `s`, otherwise return `false`.

An anagram means:

* Both strings contain the same characters
* Same frequency of each character
* Order does not matter

---

### 📌 Examples

```id="ex1"
Input: s = "anagram", t = "nagaram"
Output: true
```

```id="ex2"
Input: s = "rat", t = "car"
Output: false
```

---

# 🔴 1. Brute Force (Sorting)

### 💡 Idea

If two strings are anagrams:

* After sorting both strings
* They must be equal

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.size() != t.size()) return false;

        sort(s.begin(), s.end());
        sort(t.begin(), t.end());

        return s == t;
    }
};
```

---

### ⏱ Time Complexity

O(N log N)

### 🗂 Space Complexity

O(1) (ignoring sort internal space)

❌ Sorting is unnecessary work.

---

# 🟡 2. Better Solution (Using HashMap)

### 💡 Idea

Count character frequencies using `unordered_map`.

1. Increase count for characters in `s`
2. Decrease count for characters in `t`
3. If any count becomes negative → not anagram

---

### 💻 Code (C++)

```cpp id="map1"
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.size() != t.size()) return false;

        unordered_map<char, int> mp;

        for(char c : s)
            mp[c]++;

        for(char c : t) {
            mp[c]--;
            if(mp[c] < 0)
                return false;
        }

        return true;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(K) (K = unique characters)

✅ Works for general Unicode characters.

---

# 🟢 3. Optimum Solution (Fixed Size Frequency Array)

### 💡 Idea

Since problem usually states:

* Strings contain only lowercase English letters

Use fixed array of size 26.

Faster than hashmap.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.size() != t.size()) return false;

        int freq[26] = {0};

        for(int i = 0; i < s.size(); i++) {
            freq[s[i] - 'a']++;
            freq[t[i] - 'a']--;
        }

        for(int i = 0; i < 26; i++) {
            if(freq[i] != 0)
                return false;
        }

        return true;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1) (constant 26 size array)

⭐ Most optimal for lowercase letters.

---

# 🎯 Interview Insight

* If character set is small → use array
* If Unicode allowed → use hashmap
* Always check length first

---

# 🏆 Final Comparison

| Approach        | Time       | Space | Recommended |
| --------------- | ---------- | ----- | ----------- |
| Sorting         | O(N log N) | O(1)  | ❌           |
| HashMap         | O(N)       | O(K)  | ✅           |
| Frequency Array | O(N)       | O(1)  | ⭐ Best      |

---

