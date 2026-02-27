# [Longest Common Prefix](#longest-common-prefix)



## 🧩 Problem: Longest Common Prefix

Write a function to find the **longest common prefix string** amongst an array of strings.

If there is no common prefix, return an empty string `""`.

---

## 🔎 Examples

**Example 1:**

```id="ex1"
Input: strs = ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**

```id="ex2"
Input: strs = ["dog","racecar","car"]
Output: ""
```

**Example 3:**

```id="ex3"
Input: strs = ["interspecies","interstellar","interstate"]
Output: "inters"
```

---

# 🐢 Brute Force Solution (Horizontal Scanning)

### 💡 Idea

Take the first string as prefix.
Compare it with every other string and shrink the prefix until it matches.

---

### 🔹 Algorithm

1. If array is empty → return `""`
2. Set `prefix = strs[0]`
3. For each string `str`:

   * While `str.find(prefix) != 0`

     * Remove last character from prefix
4. Return prefix

---

### ⏱ Time Complexity

* **O(n * m²)**
  (n = number of strings, m = length of prefix)

### 📦 Space Complexity

* **O(1)**

---

### 💻 C++ Code

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.empty()) return "";

        string prefix = strs[0];

        for(int i = 1; i < strs.size(); i++) {
            while(strs[i].find(prefix) != 0) {
                prefix.pop_back();
                if(prefix.empty()) return "";
            }
        }

        return prefix;
    }
};
```

---

# 🚀 Better Solution (Vertical Scanning)

### 💡 Idea

Compare characters column-wise.

Instead of shrinking prefix repeatedly:

* Compare characters at index `i` for all strings.
* Stop when mismatch found.

---

### 🔹 Algorithm

1. Iterate `i` from `0` to length of first string
2. For each string:

   * If `i` exceeds string length OR characters mismatch → return substring(0, i)
3. If loop completes → first string is prefix

---

### ⏱ Time Complexity

* **O(n * m)**

### 📦 Space Complexity

* **O(1)**

---

### 💻 C++ Code

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.empty()) return "";

        for(int i = 0; i < strs[0].size(); i++) {
            char c = strs[0][i];

            for(int j = 1; j < strs.size(); j++) {
                if(i >= strs[j].size() || strs[j][i] != c) {
                    return strs[0].substr(0, i);
                }
            }
        }

        return strs[0];
    }
};
```

---

# ⚡ Optimum Solution (Sorting Trick)

### 💡 Key Observation

After sorting:

* Only the **first and last strings** matter.
* The common prefix of the whole array =
  common prefix of first & last string.

---

### 🔹 Algorithm

1. Sort array
2. Compare first and last string
3. Find common prefix between them

---

### ⏱ Time Complexity

* Sorting: **O(n log n)**
* Comparing: **O(m)**
* Total: **O(n log n)**

### 📦 Space Complexity

* **O(1)** (if sorting in-place)

---

### 💻 C++ Code

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.empty()) return "";

        sort(strs.begin(), strs.end());

        string first = strs.front();
        string last = strs.back();
        int i = 0;

        while(i < first.size() && first[i] == last[i]) {
            i++;
        }

        return first.substr(0, i);
    }
};
```

---

# 🏆 Final Recommendation

| Approach            | Time         | Space | Recommendation   |
| ------------------- | ------------ | ----- | ---------------- |
| Horizontal Scanning | O(n * m²)    | O(1)  | ❌ Slower         |
| Vertical Scanning   | **O(n * m)** | O(1)  | ⭐ Best           |
| Sorting Trick       | O(n log n)   | O(1)  | Good alternative |

👉 In interviews, prefer **Vertical Scanning** — clean, optimal for most cases.

---
