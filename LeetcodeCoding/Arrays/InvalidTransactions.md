# [Invalid Transactions](#invalid-transactions)

## 🟢 Invalid Transactions

---

### 📌 Problem

A transaction is invalid if:

1. The amount exceeds `1000`, **or**
2. It occurs within (or equal to) 60 minutes of another transaction with the **same name** but in a **different city**.

Each transaction is given as a string:

```id="fmt"
"name,time,amount,city"
```

Return a list of all invalid transactions.

---

### 📌 Example

```id="ex1"
Input:
transactions = ["alice,20,800,mtv","alice,50,100,beijing"]

Output:
["alice,20,800,mtv","alice,50,100,beijing"]
```

Reason:

* Same name
* Within 60 minutes
* Different cities

---

# 🔴 1. Brute Force (Compare Every Pair)

### 💡 Idea

For every transaction:

* Parse it
* Compare with every other transaction
* Mark invalid if:

  * Amount > 1000
  * Same name, |time difference| ≤ 60, different city

---

### 🔹 Algorithm

1. Parse all transactions into structured form
2. Use nested loops to compare pairs
3. Mark invalid using boolean array
4. Collect invalid ones

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    vector<string> invalidTransactions(vector<string>& transactions) {
        int n = transactions.size();
        vector<bool> invalid(n, false);

        vector<string> name(n), city(n);
        vector<int> time(n), amount(n);

        // Parse transactions
        for(int i = 0; i < n; i++) {
            string s = transactions[i];
            stringstream ss(s);
            string token;

            getline(ss, name[i], ',');
            getline(ss, token, ',');
            time[i] = stoi(token);
            getline(ss, token, ',');
            amount[i] = stoi(token);
            getline(ss, city[i], ',');
        }

        for(int i = 0; i < n; i++) {
            if(amount[i] > 1000)
                invalid[i] = true;

            for(int j = i + 1; j < n; j++) {
                if(name[i] == name[j] &&
                   abs(time[i] - time[j]) <= 60 &&
                   city[i] != city[j]) {

                    invalid[i] = true;
                    invalid[j] = true;
                }
            }
        }

        vector<string> result;
        for(int i = 0; i < n; i++) {
            if(invalid[i])
                result.push_back(transactions[i]);
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(N)

---

# 🟡 2. Better Solution (Group by Name)

### 💡 Idea

Instead of comparing all pairs:

* Group transactions by name using `unordered_map`
* Only compare transactions within the same name group

This reduces unnecessary comparisons.

---

### 🔹 Steps

1. Parse transactions
2. Store indices grouped by name
3. For each group:

   * Compare transactions inside that group only
4. Mark invalid

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    vector<string> invalidTransactions(vector<string>& transactions) {
        int n = transactions.size();
        vector<bool> invalid(n, false);

        vector<string> name(n), city(n);
        vector<int> time(n), amount(n);

        unordered_map<string, vector<int>> mp;

        for(int i = 0; i < n; i++) {
            stringstream ss(transactions[i]);
            string token;

            getline(ss, name[i], ',');
            getline(ss, token, ',');
            time[i] = stoi(token);
            getline(ss, token, ',');
            amount[i] = stoi(token);
            getline(ss, city[i], ',');

            mp[name[i]].push_back(i);
        }

        for(int i = 0; i < n; i++) {
            if(amount[i] > 1000)
                invalid[i] = true;
        }

        for(auto& [_, indices] : mp) {
            for(int i = 0; i < indices.size(); i++) {
                for(int j = i + 1; j < indices.size(); j++) {
                    int idx1 = indices[i];
                    int idx2 = indices[j];

                    if(abs(time[idx1] - time[idx2]) <= 60 &&
                       city[idx1] != city[idx2]) {

                        invalid[idx1] = true;
                        invalid[idx2] = true;
                    }
                }
            }
        }

        vector<string> result;
        for(int i = 0; i < n; i++) {
            if(invalid[i])
                result.push_back(transactions[i]);
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

Worst case still O(N²)
(Best case improved due to grouping)

### 🗂 Space Complexity

O(N)

---

# 🟢 3. Optimum Practical Approach (Sort by Time within Name)

### 💡 Idea

Within each name group:

* Sort transactions by time
* Only check nearby transactions within 60-minute window

This reduces unnecessary comparisons.

---

### 🔹 Steps

1. Parse transactions
2. Group by name
3. For each group:

   * Sort by time
   * For each transaction:

     * Check forward while time difference ≤ 60
4. Mark invalid

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    vector<string> invalidTransactions(vector<string>& transactions) {
        int n = transactions.size();
        vector<bool> invalid(n, false);

        vector<string> name(n), city(n);
        vector<int> time(n), amount(n);

        unordered_map<string, vector<int>> mp;

        for(int i = 0; i < n; i++) {
            stringstream ss(transactions[i]);
            string token;

            getline(ss, name[i], ',');
            getline(ss, token, ',');
            time[i] = stoi(token);
            getline(ss, token, ',');
            amount[i] = stoi(token);
            getline(ss, city[i], ',');

            mp[name[i]].push_back(i);

            if(amount[i] > 1000)
                invalid[i] = true;
        }

        for(auto& [_, indices] : mp) {
            sort(indices.begin(), indices.end(),
                [&](int a, int b) {
                    return time[a] < time[b];
                });

            for(int i = 0; i < indices.size(); i++) {
                for(int j = i + 1; j < indices.size(); j++) {
                    if(time[indices[j]] - time[indices[i]] > 60)
                        break;

                    if(city[indices[i]] != city[indices[j]]) {
                        invalid[indices[i]] = true;
                        invalid[indices[j]] = true;
                    }
                }
            }
        }

        vector<string> result;
        for(int i = 0; i < n; i++) {
            if(invalid[i])
                result.push_back(transactions[i]);
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

Worst case: O(N²)
Practical case: Much faster due to pruning

### 🗂 Space Complexity

O(N)

---

# 🏆 Final Comparison

| Approach          | Time        | Space | Recommended      |
| ----------------- | ----------- | ----- | ---------------- |
| Full Pair Compare | O(N²)       | O(N)  | ❌                |
| Group by Name     | O(N²)       | O(N)  | ✅                |
| Sort + Window     | O(N²) worst | O(N)  | ⭐ Best Practical |

---

