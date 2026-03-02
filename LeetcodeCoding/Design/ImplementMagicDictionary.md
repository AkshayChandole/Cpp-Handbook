# [Implement Magic Dictionary](#implement-magic-dictionary)


## 🟢 Implement Magic Dictionary

---

### 📌 Problem

Design a data structure that supports:

* `buildDict(dictionary)` → build dictionary using list of words.
* `search(searchWord)` → return `true` if there exists a word in dictionary such that:

  * It differs by **exactly one character**
  * Length must be the same

---

### 📌 Example

```id="ex1"
Input:
["MagicDictionary","buildDict","search","search","search","search"]
[[],[["hello","leetcode"]],["hello"],["hhllo"],["hell"],["leetcoded"]]

Output:
[null,null,false,true,false,false]
```

Explanation:

* `"hello"` → false (no modification)
* `"hhllo"` → true (one character difference from `"hello"`)
* `"hell"` → false (different length)
* `"leetcoded"` → false

---

# 🔴 1. Brute Force (Compare With All Words)

### 💡 Idea

For each search:

* Check all words in dictionary.
* Count differing characters.
* If exactly 1 → return true.

---

### 💻 Code (C++)

```cpp id="bf1"
class MagicDictionary {
public:
    vector<string> dict;

    void buildDict(vector<string> dictionary) {
        dict = dictionary;
    }

    bool search(string searchWord) {
        for(string word : dict) {
            if(word.size() != searchWord.size())
                continue;

            int diff = 0;

            for(int i = 0; i < word.size(); i++) {
                if(word[i] != searchWord[i])
                    diff++;

                if(diff > 1)
                    break;
            }

            if(diff == 1)
                return true;
        }

        return false;
    }
};
```

---

### ⏱ Time Complexity

Let:

* N = number of words
* M = word length

Search → O(N × M)

### 🗂 Space Complexity

O(N × M)

❌ Slow for large dictionary.

---

# 🟡 2. Better Solution (Precompute Patterns with Wildcards)

### 💡 Key Insight

For word `"hello"`:

Generate patterns:

```id="patterns"
*ello
h*llo
he*lo
hel*o
hell*
```

Store these patterns in hashmap.

If a pattern appears more than once OR
pattern exists but original word is different → valid.

---

### 💻 Code (C++)

```cpp id="better1"
class MagicDictionary {
public:
    unordered_map<string, int> mp;
    unordered_set<string> words;

    void buildDict(vector<string> dictionary) {
        for(string word : dictionary) {
            words.insert(word);

            for(int i = 0; i < word.size(); i++) {
                string pattern = word;
                pattern[i] = '*';
                mp[pattern]++;
            }
        }
    }

    bool search(string searchWord) {
        for(int i = 0; i < searchWord.size(); i++) {
            string pattern = searchWord;
            pattern[i] = '*';

            if(mp.count(pattern)) {
                if(mp[pattern] > 1 || !words.count(searchWord))
                    return true;
            }
        }

        return false;
    }
};
```

---

### ⏱ Time Complexity

Build → O(N × M)
Search → O(M)

### 🗂 Space Complexity

O(N × M)

✅ Much faster search.

---

# 🟢 3. Optimum Solution (Trie + DFS with One Modification)

### 💡 Idea

Use Trie.

During search:

* At each character:

  * Either match same character.
  * Or (if not modified yet) try all other characters.
* Ensure exactly one modification used.

---

### 💻 Code (C++)

```cpp id="opt1"
class TrieNode {
public:
    bool isEnd;
    unordered_map<char, TrieNode*> children;
    TrieNode() { isEnd = false; }
};

class MagicDictionary {
public:
    TrieNode* root;

    MagicDictionary() {
        root = new TrieNode();
    }

    void buildDict(vector<string> dictionary) {
        for(string word : dictionary) {
            TrieNode* node = root;
            for(char c : word) {
                if(!node->children.count(c))
                    node->children[c] = new TrieNode();
                node = node->children[c];
            }
            node->isEnd = true;
        }
    }

    bool dfs(string& word, int index,
             TrieNode* node, bool modified) {

        if(index == word.size())
            return modified && node->isEnd;

        for(auto& [ch, child] : node->children) {
            if(ch == word[index]) {
                if(dfs(word, index + 1, child, modified))
                    return true;
            }
            else if(!modified) {
                if(dfs(word, index + 1, child, true))
                    return true;
            }
        }

        return false;
    }

    bool search(string searchWord) {
        return dfs(searchWord, 0, root, false);
    }
};
```

---

### ⏱ Time Complexity

Search → O(26 × M) worst case

### 🗂 Space Complexity

O(N × M)

⭐ Best conceptual solution.

---

# 🔍 Why Exactly One Modification?

In DFS base case:

```cpp id="check"
return modified && node->isEnd;
```

Ensures:

* Word ends.
* Exactly one change used.

---

# 🏆 Final Comparison

| Approach      | Build | Search | Space | Recommended |
| ------------- | ----- | ------ | ----- | ----------- |
| Brute Force   | O(NM) | O(NM)  | O(NM) | ❌           |
| Wildcard Hash | O(NM) | O(M)   | O(NM) | ✅           |
| Trie + DFS    | O(NM) | O(26M) | O(NM) | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* String manipulation
* Pattern hashing
* Trie + backtracking
* Handling exactly-one-modification condition

Related problems:

* Implement Trie
* Word Dictionary (Add & Search Word)
* Word Search
* Replace Words

---
