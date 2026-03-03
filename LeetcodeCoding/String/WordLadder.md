# [Word Ladder](#word-ladder)

## 🟢 Word Ladder

---

### 📌 Problem

Given:

* `beginWord`
* `endWord`
* `wordList`

Return the **length of the shortest transformation sequence** from `beginWord` to `endWord`.

Rules:

1. Only one letter can be changed at a time.
2. Each transformed word must exist in `wordList`.

If no transformation possible → return `0`.

---

### 📌 Example

```id="ex1"
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log","cog"]

Output: 5

Explanation:
hit → hot → dot → dog → cog
```

---

```id="ex2"
Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: 0
```

---

# 🔴 1. Brute Force (Generate All Paths – DFS)

---

### 💡 Idea

* Try all possible transformations.
* Recursively search all paths.
* Track minimum length.

---

### ❌ Why It Fails

* Huge branching factor.
* Exponential time.
* TLE for large inputs.

---

### ⏱ Time Complexity

O(N!) worst case

❌ Not acceptable.

---

# 🟡 2. Better Solution (Standard BFS)

---

### 💡 Key Insight

This is a **shortest path problem in an unweighted graph**.

* Each word = node
* Edge exists if words differ by one letter
* Use BFS

---

### 🔹 Steps

1. Put all words in `unordered_set`.
2. Use queue for BFS.
3. For each word:

   * Try changing each character (`a → z`)
   * If valid word in set:

     * Add to queue
     * Remove from set (avoid revisit)

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int ladderLength(string beginWord,
                     string endWord,
                     vector<string>& wordList) {

        unordered_set<string> wordSet(
            wordList.begin(), wordList.end()
        );

        if(!wordSet.count(endWord))
            return 0;

        queue<string> q;
        q.push(beginWord);

        int level = 1;

        while(!q.empty()) {
            int size = q.size();

            while(size--) {
                string word = q.front();
                q.pop();

                if(word == endWord)
                    return level;

                for(int i = 0; i < word.size(); i++) {
                    char original = word[i];

                    for(char c = 'a'; c <= 'z'; c++) {
                        word[i] = c;

                        if(wordSet.count(word)) {
                            q.push(word);
                            wordSet.erase(word);
                        }
                    }

                    word[i] = original;
                }
            }

            level++;
        }

        return 0;
    }
};
```

---

### ⏱ Time Complexity

O(N × L × 26)

Where:

* N = number of words
* L = word length

### 🗂 Space Complexity

O(N)

✅ Standard solution.

---

# 🟢 3. Optimum Solution (Bidirectional BFS)

---

### 💡 Why Optimize?

Standard BFS:

* Explores from beginWord outward.
* May explore many unnecessary nodes.

Better approach:

> Start BFS from BOTH ends.

---

### 🔹 Idea

* Maintain two sets:

  * `beginSet`
  * `endSet`
* Expand smaller set each time.
* Stop when sets intersect.

This reduces search space significantly.

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int ladderLength(string beginWord,
                     string endWord,
                     vector<string>& wordList) {

        unordered_set<string> wordSet(
            wordList.begin(), wordList.end()
        );

        if(!wordSet.count(endWord))
            return 0;

        unordered_set<string> beginSet{beginWord};
        unordered_set<string> endSet{endWord};

        int level = 1;

        while(!beginSet.empty() && !endSet.empty()) {

            if(beginSet.size() > endSet.size())
                swap(beginSet, endSet);

            unordered_set<string> nextLevel;

            for(string word : beginSet) {
                for(int i = 0; i < word.size(); i++) {
                    char original = word[i];

                    for(char c = 'a'; c <= 'z'; c++) {
                        word[i] = c;

                        if(endSet.count(word))
                            return level + 1;

                        if(wordSet.count(word)) {
                            nextLevel.insert(word);
                            wordSet.erase(word);
                        }
                    }

                    word[i] = original;
                }
            }

            beginSet = nextLevel;
            level++;
        }

        return 0;
    }
};
```

---

### ⏱ Time Complexity

O(N × L)

Much faster in practice.

### 🗂 Space Complexity

O(N)

⭐ Best solution.

---

# 🔍 Why BFS?

Because:

* We need shortest transformation.
* All edges weight = 1.
* BFS guarantees shortest path.

---

# 🔎 Dry Run

Example:

```id="dry1"
hit → cog
```

Graph:

```id="dry2"
hit
 ↓
hot
 ↓
dot → dog
 ↓       ↓
lot → log → cog
```

Shortest length = 5

---

# 🏆 Final Comparison

| Approach          | Time          | Space | Recommended |
| ----------------- | ------------- | ----- | ----------- |
| DFS               | Exponential   | High  | ❌           |
| BFS               | O(N × L × 26) | O(N)  | ✅           |
| Bidirectional BFS | O(N × L)      | O(N)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* BFS
* Graph modeling
* String transformation
* Bidirectional search optimization

Related problems:

* Word Ladder II
* Minimum Genetic Mutation
* Open the Lock
* Shortest Path in Binary Matrix

---
