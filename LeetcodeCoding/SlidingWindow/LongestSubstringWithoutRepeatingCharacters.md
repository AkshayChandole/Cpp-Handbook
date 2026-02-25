# [Longest Substring Without Repeating Characters](#longest-substring-without-repeating-characters)

## 🧩 Problem: Longest Substring Without Repeating Characters

Given a string `s`, find the **length of the longest substring** without repeating characters.

* A substring is a contiguous sequence of characters.
* Characters must not repeat within the substring.

---

## 🔎 Examples

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: "abc" is the longest substring.
```

**Example 2:**

```
Input: s = "bbbbb"
Output: 1
Explanation: "b" is the longest substring.
```

**Example 3:**

```
Input: s = "pwwkew"
Output: 3
Explanation: "wke" is the longest substring.
```

---

# 🐢 Brute Force Solution

### 💡 Idea

Generate all substrings and check if each substring contains unique characters.

### 🔹 Algorithm

1. Generate all substrings using two loops.
2. For each substring, check if characters are unique (using set or frequency array).
3. Track maximum length.

### ⏱ Time Complexity

* Generating substrings: **O(n²)**
* Checking uniqueness: **O(n)**
* Total: **O(n³)**

### 📦 Space Complexity

* **O(n)** (for set)

### 💻 C++ Code

```cpp
class Solution {
public:
    bool allUnique(string s, int start, int end) {
        vector<bool> visited(256, false);
        for(int i = start; i <= end; i++) {
            if(visited[s[i]]) return false;
            visited[s[i]] = true;
        }
        return true;
    }

    int lengthOfLongestSubstring(string s) {
        int n = s.length();
        int maxLen = 0;

        for(int i = 0; i < n; i++) {
            for(int j = i; j < n; j++) {
                if(allUnique(s, i, j)) {
                    maxLen = max(maxLen, j - i + 1);
                }
            }
        }
        return maxLen;
    }
};
```

---

# 🚀 Better Solution (Sliding Window + Set)

### 💡 Idea

Use two pointers and maintain a set of characters in the current window.

If duplicate found:

* Remove characters from left until duplicate removed.

### 🔹 Algorithm

1. Maintain `left = 0`
2. Iterate `right` from 0 → n-1
3. If `s[right]` not in set:

   * Insert it
   * Update max length
4. Else:

   * Remove `s[left]` and increment left
   * Repeat until duplicate removed

### ⏱ Time Complexity

* Each character inserted & removed once → **O(2n) ≈ O(n)**

### 📦 Space Complexity

* **O(n)** (set storage)

### 💻 C++ Code

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_set<char> st;
        int left = 0, maxLen = 0;

        for(int right = 0; right < s.size(); right++) {
            while(st.find(s[right]) != st.end()) {
                st.erase(s[left]);
                left++;
            }
            st.insert(s[right]);
            maxLen = max(maxLen, right - left + 1);
        }

        return maxLen;
    }
};
```

---

# ⚡ Optimum Solution (Sliding Window + Hash Map)

### 💡 Idea

Instead of removing characters one by one, directly jump `left`
to the position after the last occurrence of duplicate.

This avoids unnecessary removals.

### 🔹 Algorithm

1. Use `unordered_map<char, int>` to store last index of each character.
2. Iterate `right` from 0 → n-1
3. If character seen before:

   * Move `left = max(left, lastIndex + 1)`
4. Update character's last index
5. Update maximum length

### ⏱ Time Complexity

* **O(n)** — single pass

### 📦 Space Complexity

* **O(n)** — hashmap storage

### 💻 C++ Code

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char, int> mp;
        int left = 0, maxLen = 0;

        for(int right = 0; right < s.size(); right++) {
            if(mp.find(s[right]) != mp.end()) {
                left = max(left, mp[s[right]] + 1);
            }

            mp[s[right]] = right;
            maxLen = max(maxLen, right - left + 1);
        }

        return maxLen;
    }
};
```

---

# 🏆 Final Recommendation

| Approach                 | Time     | Space | Recommendation  |
| ------------------------ | -------- | ----- | --------------- |
| Brute Force              | O(n³)    | O(n)  | ❌ Not practical |
| Sliding Window + Set     | O(n)     | O(n)  | ✅ Good          |
| Sliding Window + HashMap | **O(n)** | O(n)  | ⭐ Best          |

