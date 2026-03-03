# [Design Search Autocomplete System](#design-search-autocomplete-system)


## 🟢 Design Search Autocomplete System

---

### 📌 Problem

Design an autocomplete system that:

### Supports:

```cpp
AutocompleteSystem(string[] sentences, int[] times)
vector<string> input(char c)
```

### Behavior:

* Initially given:

  * `sentences[i]`
  * `times[i]` → frequency

* For each character typed:

  * Return top 3 **historical sentences**
  * Matching current prefix
  * Sorted by:

    1. Highest frequency
    2. Lexicographically smaller (ASCII order)

* If input character is `'#'`:

  * Add current sentence to history
  * Reset input buffer

---

### 📌 Example

```id="ex1"
Input:
sentences = ["i love you","island","iroman","i love leetcode"]
times = [5,3,2,2]

Input sequence:
i → return top 3
space → return top 3
a → return []
# → save sentence
```

---

# 🔴 1. Brute Force (HashMap + Scan Every Time)

---

### 💡 Idea

Maintain:

```cpp
unordered_map<string, int> freq;
```

On every input:

* Iterate through all keys.
* Filter matching prefix.
* Sort results.
* Return top 3.

---

### 💻 Code (C++)

```cpp
class AutocompleteSystem {
public:
    unordered_map<string, int> freq;
    string current;

    AutocompleteSystem(vector<string>& sentences,
                       vector<int>& times) {
        for(int i = 0; i < sentences.size(); i++)
            freq[sentences[i]] = times[i];
    }

    vector<string> input(char c) {
        if(c == '#') {
            freq[current]++;
            current = "";
            return {};
        }

        current += c;

        vector<pair<string,int>> matches;

        for(auto& [s, count] : freq) {
            if(s.substr(0, current.size()) == current)
                matches.push_back({s, count});
        }

        sort(matches.begin(), matches.end(),
             [](auto& a, auto& b) {
                 if(a.second == b.second)
                     return a.first < b.first;
                 return a.second > b.second;
             });

        vector<string> result;
        for(int i = 0; i < min(3, (int)matches.size()); i++)
            result.push_back(matches[i].first);

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N log N) per input
(N = total sentences)

❌ Too slow for large dataset.

---

# 🟡 2. Better Solution (Trie + Store Sentences at Each Node)

---

### 💡 Key Idea

Use **Trie**.

Each node stores:

```cpp
unordered_map<string,int> counts;
```

Meaning:

* For each prefix node,
* Store all sentences passing through it.

Then:

* On input:

  * Traverse trie to prefix node.
  * Get all sentences.
  * Sort top 3.

---

### 🔹 Structure

```cpp
class TrieNode {
public:
    unordered_map<char, TrieNode*> children;
    unordered_map<string, int> counts;
};
```

---

### 💻 Code (C++ Simplified)

```cpp
class TrieNode {
public:
    unordered_map<char, TrieNode*> children;
    unordered_map<string, int> counts;
};

class AutocompleteSystem {
public:
    TrieNode* root;
    string current;

    AutocompleteSystem(vector<string>& sentences,
                       vector<int>& times) {
        root = new TrieNode();
        for(int i = 0; i < sentences.size(); i++)
            addSentence(sentences[i], times[i]);
    }

    void addSentence(string sentence, int time) {
        TrieNode* node = root;
        for(char c : sentence) {
            if(!node->children[c])
                node->children[c] = new TrieNode();
            node = node->children[c];
            node->counts[sentence] += time;
        }
    }

    vector<string> input(char c) {
        if(c == '#') {
            addSentence(current, 1);
            current = "";
            return {};
        }

        current += c;
        TrieNode* node = root;

        for(char ch : current) {
            if(!node->children.count(ch))
                return {};
            node = node->children[ch];
        }

        vector<pair<string,int>> candidates(
            node->counts.begin(),
            node->counts.end()
        );

        sort(candidates.begin(), candidates.end(),
             [](auto& a, auto& b) {
                 if(a.second == b.second)
                     return a.first < b.first;
                 return a.second > b.second;
             });

        vector<string> result;
        for(int i = 0; i < min(3, (int)candidates.size()); i++)
            result.push_back(candidates[i].first);

        return result;
    }
};
```

---

### ⏱ Time Complexity

Insert: O(L)
Search: O(P + M log M)

Where:

* L = sentence length
* P = prefix length
* M = matching sentences

Better than brute force.

---

# 🟢 3. Optimum Solution (Trie + Top 3 Stored at Each Node)

---

### 💡 Ultimate Optimization

Instead of storing all sentences at node:

Store only **top 3** sentences at each node.

So:

* No sorting during query.
* Just return stored list.

---

### 🔹 Node Structure

```cpp
class TrieNode {
public:
    unordered_map<char, TrieNode*> children;
    vector<string> top3;
};
```

We also maintain global `freq`.

When inserting:

* Update top3 at each prefix node.
* Keep it sorted and trimmed to 3.

---

### 💻 Code (Optimized C++)

```cpp
class TrieNode {
public:
    unordered_map<char, TrieNode*> children;
    vector<string> top3;
};

class AutocompleteSystem {
public:
    TrieNode* root;
    unordered_map<string,int> freq;
    string current;

    AutocompleteSystem(vector<string>& sentences,
                       vector<int>& times) {
        root = new TrieNode();
        for(int i = 0; i < sentences.size(); i++) {
            freq[sentences[i]] = times[i];
            insert(sentences[i]);
        }
    }

    void insert(string sentence) {
        TrieNode* node = root;

        for(char c : sentence) {
            if(!node->children[c])
                node->children[c] = new TrieNode();

            node = node->children[c];

            node->top3.push_back(sentence);

            sort(node->top3.begin(), node->top3.end(),
                [&](string& a, string& b) {
                    if(freq[a] == freq[b])
                        return a < b;
                    return freq[a] > freq[b];
                });

            if(node->top3.size() > 3)
                node->top3.pop_back();
        }
    }

    vector<string> input(char c) {
        if(c == '#') {
            freq[current]++;
            insert(current);
            current = "";
            return {};
        }

        current += c;
        TrieNode* node = root;

        for(char ch : current) {
            if(!node->children.count(ch))
                return {};
            node = node->children[ch];
        }

        return node->top3;
    }
};
```

---

### ⏱ Time Complexity

Insert: O(L × 3 log 3) ≈ O(L)
Search: O(P)

⭐ Best solution.

---

# 🏆 Final Comparison

| Approach           | Query Time | Space  | Recommended |
| ------------------ | ---------- | ------ | ----------- |
| Brute Force        | O(N log N) | O(N)   | ❌           |
| Trie + Full Sort   | O(M log M) | High   | ⚠           |
| Trie + Top3 Cached | O(P)       | Higher | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Trie data structure
* Custom sorting
* System design thinking
* Prefix-based search
* Performance optimization

Related problems:

* Implement Trie
* Word Dictionary
* Design Twitter
* Top K Frequent Words

---

