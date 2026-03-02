
# [Group Anagrams](#Group-Anagrams)


## 🟢 Group Anagrams

---

### 📌 Problem

Given an array of strings `strs`, group the **anagrams** together.

Return the groups in any order.

Two strings are anagrams if:

* They contain the same characters
* With the same frequency
* Order does not matter

---

### 📌 Examples

```id="ex1"
Input: strs = ["eat","tea","tan","ate","nat","bat"]

Output:
[
  ["eat","tea","ate"],
  ["tan","nat"],
  ["bat"]
]
```

```id="ex2"
Input: strs = [""]
Output: [[""]]
```

```id="ex3"
Input: strs = ["a"]
Output: [["a"]]
```

---

# 🔴 1. Brute Force (Compare Each String with Others)

### 💡 Idea

For each string:

* Compare with every other string
* Check if they are anagrams (by sorting or frequency)
* Group them

---

### 💻 Code (C++ – Inefficient)

```cpp id="bf1"
class Solution {
public:
    bool isAnagram(string a, string b) {
        if(a.size() != b.size()) return false;
        sort(a.begin(), a.end());
        sort(b.begin(), b.end());
        return a == b;
    }

    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> result;
        vector<bool> visited(strs.size(), false);

        for(int i = 0; i < strs.size(); i++) {
            if(visited[i]) continue;

            vector<string> group;
            group.push_back(strs[i]);
            visited[i] = true;

            for(int j = i + 1; j < strs.size(); j++) {
                if(!visited[j] && isAnagram(strs[i], strs[j])) {
                    group.push_back(strs[j]);
                    visited[j] = true;
                }
            }

            result.push_back(group);
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N² * K log K)

Where:

* `N` = number of strings
* `K` = max string length

❌ Too slow.

---

# 🟡 2. Better Solution (Sorting Each String as Key)

### 💡 Idea

Anagrams become identical after sorting.

Use:

```id="key1"
unordered_map<string, vector<string>>
```

* Key → sorted string
* Value → list of anagrams

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> mp;

        for(string s : strs) {
            string key = s;
            sort(key.begin(), key.end());
            mp[key].push_back(s);
        }

        vector<vector<string>> result;
        for(auto& pair : mp) {
            result.push_back(pair.second);
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N * K log K)

### 🗂 Space Complexity

O(N * K)

✅ Most common solution.

---

# 🟢 3. Optimum Solution (Character Frequency as Key)

### 💡 Key Insight

Instead of sorting (K log K):

* Count frequency of 26 letters.
* Use frequency signature as key.

This avoids sorting.

---

### 🔹 Example Key

For `"eat"`:

```id="sig"
[1,0,0,0,1,0,...,1,...]
```

Represent as string:

```id="sig2"
"1#0#0#0#1#...#1#..."
```

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> mp;

        for(string s : strs) {
            vector<int> freq(26, 0);

            for(char c : s)
                freq[c - 'a']++;

            string key;
            for(int count : freq) {
                key += to_string(count) + "#";
            }

            mp[key].push_back(s);
        }

        vector<vector<string>> result;
        for(auto& pair : mp) {
            result.push_back(pair.second);
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N * K)

### 🗂 Space Complexity

O(N * K)

⭐ Most optimal approach.

---

# 🔍 Why Frequency Key Is Better?

Sorting costs:

```id="sortcost"
O(K log K)
```

Frequency counting costs:

```id="freqcost"
O(K)
```

So overall:

| Method    | Time         |
| --------- | ------------ |
| Sorting   | O(N K log K) |
| Frequency | O(N K)       |

---

# 🎯 Interview Insight

This problem teaches:

* Hashing with custom keys
* Signature transformation
* Frequency counting optimization

Related problems:

* Valid Anagram
* Find All Anagrams in a String
* Group Shifted Strings

---

# 🏆 Final Comparison

| Approach      | Time          | Space | Recommended |
| ------------- | ------------- | ----- | ----------- |
| Brute Force   | O(N² K log K) | O(N)  | ❌           |
| Sorting Key   | O(N K log K)  | O(NK) | ✅           |
| Frequency Key | O(NK)         | O(NK) | ⭐ Best      |

---

