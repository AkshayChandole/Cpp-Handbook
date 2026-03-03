# [Bus Routes](#Bus-Routes)

## 🟢 Bus Routes

---

### 📌 Problem

You are given:

* `routes[i]` → bus route of bus `i`
* Each route is a circular list of bus stops.

You start at stop `source` and want to reach `target`.

Return the **minimum number of buses** you must take to reach `target`.

If impossible → return `-1`.

---

### 📌 Example

```id="ex1"
Input:
routes = [[1,2,7],[3,6,7]]
source = 1
target = 6

Output: 2

Explanation:
Take bus 0 → stop 7
Then bus 1 → stop 6
```

---

```id="ex2"
Input:
routes = [[7,12],[4,5,15],[6],[15,19],[9,12,13]]
source = 15
target = 12

Output: -1
```

---

# 🔴 1. Brute Force (DFS over Stops)

---

### 💡 Idea

* Try all possible routes recursively.
* Explore all reachable stops.
* Track minimum buses.

---

### ❌ Why It Fails

* Huge branching.
* Repeated revisits.
* Exponential complexity.

---

### ⏱ Time Complexity

Exponential

❌ Not acceptable.

---

# 🟡 2. Better Solution (BFS on Stops)

---

### 💡 Key Insight

This is a shortest path problem.

But:

* Nodes = bus stops
* Edges = same bus route

Better modeling:

Instead of building full graph:

* Map stop → list of buses that visit it.
* BFS starting from source stop.

---

### 🔹 Steps

1. Build:

```id="map"
unordered_map<int, vector<int>> stopToBus
```

2. BFS queue → current stops
3. Maintain:

   * visitedStops
   * visitedBuses
4. For each stop:

   * For each bus serving that stop:

     * Visit all stops on that bus

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    int numBusesToDestination(vector<vector<int>>& routes,
                              int source,
                              int target) {

        if(source == target)
            return 0;

        unordered_map<int, vector<int>> stopToBus;

        for(int i = 0; i < routes.size(); i++) {
            for(int stop : routes[i]) {
                stopToBus[stop].push_back(i);
            }
        }

        queue<int> q;
        unordered_set<int> visitedStops;
        vector<bool> visitedBus(routes.size(), false);

        q.push(source);
        visitedStops.insert(source);

        int buses = 0;

        while(!q.empty()) {
            int size = q.size();
            buses++;

            while(size--) {
                int stop = q.front();
                q.pop();

                for(int bus : stopToBus[stop]) {
                    if(visitedBus[bus])
                        continue;

                    visitedBus[bus] = true;

                    for(int nextStop : routes[bus]) {
                        if(nextStop == target)
                            return buses;

                        if(!visitedStops.count(nextStop)) {
                            visitedStops.insert(nextStop);
                            q.push(nextStop);
                        }
                    }
                }
            }
        }

        return -1;
    }
};
```

---

### ⏱ Time Complexity

Let:

* N = total stops across all routes

Time ≈ O(N)

Each bus processed once.

### 🗂 Space Complexity

O(N)

✅ Efficient solution.

---

# 🟢 3. Optimum Insight (Graph of Buses Instead of Stops)

---

### 💡 Even Better Modeling

Instead of BFS on stops:

Think:

* Nodes = buses
* Edge between buses if they share a stop.

Then:

* Start from buses containing `source`
* Target buses = buses containing `target`
* BFS on buses

This avoids revisiting large stop lists repeatedly.

---

### 🔹 Why It Works

* We only care about number of buses taken.
* Not actual stop sequence.

---

### ⏱ Complexity

Similar O(N), but cleaner conceptual model.

---

# 🔍 Dry Run

Example:

```id="dry1"
routes = [[1,2,7],[3,6,7]]
source = 1
target = 6
```

Mapping:

```id="dry2"
1 → bus0
2 → bus0
7 → bus0, bus1
3 → bus1
6 → bus1
```

BFS:

1. Start at stop 1
2. Take bus0 → stops {1,2,7}
3. From 7 → bus1
4. Bus1 → stop 6

Answer = 2

---

# 🎯 Important Edge Cases

| Case                 | Result |
| -------------------- | ------ |
| source == target     | 0      |
| target unreachable   | -1     |
| source not in routes | -1     |

---

# 🏆 Final Comparison

| Approach     | Time        | Space | Recommended     |
| ------------ | ----------- | ----- | --------------- |
| DFS          | Exponential | High  | ❌               |
| BFS on Stops | O(N)        | O(N)  | ⭐ Best          |
| BFS on Buses | O(N)        | O(N)  | ⭐ Clean Concept |

---

# 🎯 Interview Insight

This problem tests:

* Graph modeling
* BFS
* Multi-source traversal
* State marking (visited buses vs stops)

Related problems:

* Word Ladder
* Open the Lock
* Minimum Genetic Mutation
* Rotting Oranges

---
