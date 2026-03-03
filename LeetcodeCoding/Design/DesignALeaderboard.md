# [Design A Leaderboard](#design-a-leaderboard)

## 🟢 Design A Leaderboard

---

### 📌 Problem

Design a leaderboard system that supports:

1. `Leaderboard()`
2. `addScore(playerId, score)`

   * Add score to player.
3. `top(K)`

   * Return sum of top K scores.
4. `reset(playerId)`

   * Reset player’s score to 0.

Constraints:

* Players can receive multiple score updates.
* Need efficient `top(K)`.

---

### 📌 Example

```id="ex1"
Input:
["Leaderboard","addScore","addScore","addScore","addScore","addScore",
 "top","reset","reset","addScore","top"]
[[],[1,73],[2,56],[3,39],[4,51],[5,4],
 [1],[1],[2],[2,51],[3]]

Output:
[null,null,null,null,null,null,
 73,null,null,null,141]
```

---

# 🔴 1. Brute Force (HashMap + Sort Every Time)

### 💡 Idea

* Store scores in `unordered_map<playerId, score>`
* For `top(K)`:

  * Copy scores to vector
  * Sort descending
  * Sum top K

---

### 💻 Code (C++)

```cpp id="bf1"
class Leaderboard {
public:
    unordered_map<int, int> scoreMap;

    void addScore(int playerId, int score) {
        scoreMap[playerId] += score;
    }

    int top(int K) {
        vector<int> scores;

        for(auto& [id, score] : scoreMap)
            scores.push_back(score);

        sort(scores.begin(), scores.end(), greater<int>());

        int sum = 0;
        for(int i = 0; i < K; i++)
            sum += scores[i];

        return sum;
    }

    void reset(int playerId) {
        scoreMap[playerId] = 0;
    }
};
```

---

### ⏱ Time Complexity

* `addScore()` → O(1)
* `top(K)` → O(N log N)
* `reset()` → O(1)

❌ Too slow for frequent `top()` calls.

---

# 🟡 2. Better Solution (Min Heap for top K)

### 💡 Idea

For `top(K)`:

* Maintain min-heap of size K
* Iterate through all scores
* Keep only top K

---

### 💻 Code (C++)

```cpp id="better1"
class Leaderboard {
public:
    unordered_map<int, int> scoreMap;

    void addScore(int playerId, int score) {
        scoreMap[playerId] += score;
    }

    int top(int K) {
        priority_queue<int, vector<int>, greater<int>> minHeap;

        for(auto& [id, score] : scoreMap) {
            minHeap.push(score);
            if(minHeap.size() > K)
                minHeap.pop();
        }

        int sum = 0;
        while(!minHeap.empty()) {
            sum += minHeap.top();
            minHeap.pop();
        }

        return sum;
    }

    void reset(int playerId) {
        scoreMap[playerId] = 0;
    }
};
```

---

### ⏱ Time Complexity

* `top(K)` → O(N log K)

Better than full sort.

---

# 🟢 3. Optimum Solution (HashMap + Multiset)

### 💡 Best Approach

Maintain:

1. `unordered_map<int,int> scoreMap`
2. `multiset<int>` to store all scores

Why multiset?

* Keeps scores sorted automatically.
* Supports:

  * Insert → O(log N)
  * Erase → O(log N)
  * Reverse iteration for top K.

---

### 💻 Code (C++)

```cpp id="opt1"
class Leaderboard {
public:
    unordered_map<int, int> scoreMap;
    multiset<int> scores;

    Leaderboard() {}

    void addScore(int playerId, int score) {
        if(scoreMap.count(playerId)) {
            scores.erase(scores.find(scoreMap[playerId]));
        }

        scoreMap[playerId] += score;
        scores.insert(scoreMap[playerId]);
    }

    int top(int K) {
        int sum = 0;
        auto it = scores.rbegin();

        while(K-- && it != scores.rend()) {
            sum += *it;
            ++it;
        }

        return sum;
    }

    void reset(int playerId) {
        scores.erase(scores.find(scoreMap[playerId]));
        scoreMap[playerId] = 0;
    }
};
```

---

### ⏱ Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| addScore  | O(log N)   |
| top(K)    | O(K)       |
| reset     | O(log N)   |

⭐ Most efficient overall.

---

# 🔍 Why Multiset Is Ideal

* Always sorted.
* Handles duplicates.
* Efficient insertion/deletion.
* Reverse iterator gives largest scores quickly.

---

# 🏆 Final Comparison

| Approach        | addScore | top(K)     | reset    | Recommended |
| --------------- | -------- | ---------- | -------- | ----------- |
| Sort Every Time | O(1)     | O(N log N) | O(1)     | ❌           |
| Min Heap        | O(1)     | O(N log K) | O(1)     | ⚠           |
| Multiset        | O(log N) | O(K)       | O(log N) | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Data structure design
* Ordered containers
* Efficient top-K queries
* Handling updates + removals

Related problems:

* Design Twitter
* Top K Frequent Elements
* LRU Cache
* Median Finder

---
