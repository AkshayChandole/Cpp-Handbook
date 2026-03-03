# [Find the Winner of the Circular Game](#find-the-winner-of-the-circular-game)

## 🟢 Find the Winner of the Circular Game

---

### 📌 Problem

There are `n` friends sitting in a circle, numbered from `1` to `n`.

Starting from friend `1`, you count `k` friends clockwise including the current one.

The `k`-th friend is eliminated.

Continue until only one friend remains.

Return the label of the winner.

---

### 📌 Example

```id="ex1"
Input: n = 5, k = 2
Output: 3

Explanation:
Friends: [1,2,3,4,5]

Remove 2 → [1,3,4,5]
Remove 4 → [1,3,5]
Remove 1 → [3,5]
Remove 5 → [3]

Winner = 3
```

```id="ex2"
Input: n = 6, k = 5
Output: 1
```

---

# 🔴 1. Brute Force (Simulation Using Vector)

### 💡 Idea

* Store players in vector.
* Maintain current index.
* Remove `(current + k - 1) % size`.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int findTheWinner(int n, int k) {
        vector<int> players;

        for(int i = 1; i <= n; i++)
            players.push_back(i);

        int index = 0;

        while(players.size() > 1) {
            index = (index + k - 1) % players.size();
            players.erase(players.begin() + index);
        }

        return players[0];
    }
};
```

---

### ⏱ Time Complexity

O(n²) (erase is O(n))

### 🗂 Space Complexity

O(n)

❌ Slow for large n.

---

# 🟡 2. Better Solution (Queue Simulation)

### 💡 Idea

Use queue:

* Rotate first `k-1` elements.
* Pop the `k`-th element.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    int findTheWinner(int n, int k) {
        queue<int> q;

        for(int i = 1; i <= n; i++)
            q.push(i);

        while(q.size() > 1) {
            for(int i = 0; i < k - 1; i++) {
                q.push(q.front());
                q.pop();
            }
            q.pop();
        }

        return q.front();
    }
};
```

---

### ⏱ Time Complexity

O(n × k)

### 🗂 Space Complexity

O(n)

✅ Cleaner but still not optimal.

---

# 🟢 3. Optimum Solution (Josephus Problem – Mathematical Recurrence)

---

### 💡 Key Insight

This is classic **Josephus Problem**.

Let:

```id="recurrence"
f(n, k) = winner position (0-indexed)
```

Recurrence:

```id="formula"
f(1, k) = 0
f(n, k) = (f(n-1, k) + k) % n
```

Convert to 1-indexed at end:

```id="result"
answer = f(n, k) + 1
```

---

### 💻 Code (C++ – Iterative)

```cpp id="opt1"
class Solution {
public:
    int findTheWinner(int n, int k) {
        int winner = 0;  // 0-indexed

        for(int i = 2; i <= n; i++) {
            winner = (winner + k) % i;
        }

        return winner + 1;  // convert to 1-indexed
    }
};
```

---

### ⏱ Time Complexity

O(n)

### 🗂 Space Complexity

O(1)

⭐ Most optimal solution.

---

# 🔍 Why Recurrence Works

Imagine:

We know winner for `n-1`.

When adding nth person:

* The position shifts by `k`.
* Take modulo to wrap around.

---

# 🔎 Dry Run

Example:

```id="dry1"
n = 5, k = 2
```

Compute:

| i | winner (0-indexed) |
| - | ------------------ |
| 1 | 0                  |
| 2 | (0+2)%2 = 0        |
| 3 | (0+2)%3 = 2        |
| 4 | (2+2)%4 = 0        |
| 5 | (0+2)%5 = 2        |

Final:

```id="dry2"
2 + 1 = 3
```

Winner = 3

---

# 🏆 Final Comparison

| Approach          | Time  | Space | Recommended |
| ----------------- | ----- | ----- | ----------- |
| Vector Simulation | O(n²) | O(n)  | ❌           |
| Queue Simulation  | O(nk) | O(n)  | ⚠           |
| Josephus Formula  | O(n)  | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem tests:

* Circular indexing
* Modular arithmetic
* Recurrence relation
* Mathematical thinking

Related problems:

* Josephus Problem
* Elimination Game
* Design Circular Queue
* Game Simulation problems

---

