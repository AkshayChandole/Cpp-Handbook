# [Top K Frequent Words](#top-k-frequent-words)
TopKFrequentWords.md

## 🟢 Top K Frequent Words

---

### 📌 Problem

Given an array of strings `words` and an integer `k`, return the `k` most frequent words.

Sorting rules:

1. Higher frequency first
2. If frequencies equal → lexicographically smaller word first

---

### 📌 Example

```id="ex1"
Input: words = ["i","love","leetcode","i","love","coding"], k = 2
Output: ["i","love"]
```

```id="ex2"
Input: words = ["the","day","is","sunny","the","the","the","sunny","is","is"], k = 4
Output: ["the","is","sunny","day"]
```

---

# 🔴 1. Brute Force (Sort Entire List)

---

### 💡 Idea

1. Count frequencies using `unordered_map`.
2. Convert to vector.
3. Sort entire vector using custom comparator.
4. Return first k.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string, int> freq;

        for(string& word : words)
            freq[word]++;

        vector<pair<string,int>> vec(freq.begin(), freq.end());

        sort(vec.begin(), vec.end(),
             [](auto& a, auto& b) {
                 if(a.second == b.second)
                     return a.first < b.first;
                 return a.second > b.second;
             });

        vector<string> result;

        for(int i = 0; i < k; i++)
            result.push_back(vec[i].first);

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N log N)

(N = unique words)

### 🗂 Space Complexity

O(N)

❌ Sorting full list unnecessary.

---

# 🟡 2. Better Solution (Min Heap of Size K)

---

### 💡 Key Insight

Use a **min heap of size k**:

Comparator:

* Smaller frequency first
* If same frequency → lexicographically larger first
  (so that worst element stays on top)

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string, int> freq;

        for(string& word : words)
            freq[word]++;

        auto comp = [](pair<string,int>& a,
                       pair<string,int>& b) {
            if(a.second == b.second)
                return a.first < b.first;  // reverse lex order
            return a.second > b.second;
        };

        priority_queue<
            pair<string,int>,
            vector<pair<string,int>>,
            decltype(comp)
        > pq(comp);

        for(auto& entry : freq) {
            pq.push(entry);
            if(pq.size() > k)
                pq.pop();
        }

        vector<string> result(k);

        for(int i = k - 1; i >= 0; i--) {
            result[i] = pq.top().first;
            pq.pop();
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N log k)

### 🗂 Space Complexity

O(N)

✅ Better for large datasets.

---

# 🟢 3. Optimum Solution (Bucket Sort)

---

### 💡 Observation

Max frequency ≤ total number of words.

We can:

1. Count frequencies.
2. Create bucket array where:

   * index = frequency
   * bucket[i] = list of words with freq i.
3. Traverse from highest frequency down.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string,int> freq;

        for(string& word : words)
            freq[word]++;

        vector<vector<string>> buckets(words.size() + 1);

        for(auto& [word, count] : freq)
            buckets[count].push_back(word);

        vector<string> result;

        for(int i = buckets.size() - 1; i >= 0 && result.size() < k; i--) {

            if(!buckets[i].empty()) {
                sort(buckets[i].begin(), buckets[i].end());

                for(string& word : buckets[i]) {
                    result.push_back(word);
                    if(result.size() == k)
                        break;
                }
            }
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N + M log M)

Where M = words in highest buckets.

Often near O(N).

### 🗂 Space Complexity

O(N)

⭐ Very efficient.

---

# 🔍 Why Heap Comparator Looks Weird?

Because we want:

* Smallest element (worst candidate) at top
* So we remove it when size > k

Heap ordering:

| Condition                            | Priority   |
| ------------------------------------ | ---------- |
| Smaller freq                         | Lower rank |
| Same freq → lexicographically larger | Lower rank |

---

# 🔎 Dry Run

Example:

```id="dry1"
["i","love","leetcode","i","love","coding"]
```

Frequency:

```id="dry2"
i → 2
love → 2
leetcode → 1
coding → 1
```

Sorted:

```id="dry3"
i, love
```

---

# 🏆 Final Comparison

| Approach    | Time       | Space | Recommended |
| ----------- | ---------- | ----- | ----------- |
| Full Sort   | O(N log N) | O(N)  | ❌           |
| Min Heap    | O(N log k) | O(N)  | ⭐           |
| Bucket Sort | O(N) avg   | O(N)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* HashMap
* Custom sorting
* Min heap usage
* Top K pattern
* Comparator logic

Related problems:

* Top K Frequent Elements
* K Closest Points to Origin
* Sort Characters By Frequency
* Design Autocomplete System

---
