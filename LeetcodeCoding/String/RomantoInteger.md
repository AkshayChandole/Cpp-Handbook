# [Roman to Integer](#roman-to-integer)

## 🟢 Roman to Integer

---

### 📌 Problem

Convert a Roman numeral string `s` into an integer.

Roman symbols:

| Symbol | Value |
| ------ | ----- |
| I      | 1     |
| V      | 5     |
| X      | 10    |
| L      | 50    |
| C      | 100   |
| D      | 500   |
| M      | 1000  |

---

### 📌 Special Rule (Subtractive Notation)

Normally symbols are added.

But if a smaller value appears before a larger value, subtract it.

Examples:

* IV → 4  (5 - 1)
* IX → 9  (10 - 1)
* XL → 40 (50 - 10)
* CM → 900 (1000 - 100)

---

### 📌 Examples

```id="ex1"
Input: s = "III"
Output: 3
```

```id="ex2"
Input: s = "LVIII"
Output: 58
Explanation: L=50, V=5, III=3
```

```id="ex3"
Input: s = "MCMXCIV"
Output: 1994
Explanation: M=1000, CM=900, XC=90, IV=4
```

---

# 🔴 1. Brute Force (Check All Special Pairs First)

### 💡 Idea

* Create map for Roman values.
* Explicitly check special pairs:

  * IV, IX, XL, XC, CD, CM
* If pair found:

  * Add special value
  * Skip next character
* Else:

  * Add single value

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<string, int> special = {
            {"IV", 4}, {"IX", 9},
            {"XL", 40}, {"XC", 90},
            {"CD", 400}, {"CM", 900}
        };

        unordered_map<char, int> mp = {
            {'I',1},{'V',5},{'X',10},{'L',50},
            {'C',100},{'D',500},{'M',1000}
        };

        int result = 0;

        for(int i = 0; i < s.size(); i++) {
            if(i + 1 < s.size()) {
                string two = s.substr(i, 2);
                if(special.count(two)) {
                    result += special[two];
                    i++;
                    continue;
                }
            }
            result += mp[s[i]];
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

Works but slightly bulky.

---

# 🟡 2. Better Solution (Compare Current and Next)

### 💡 Key Insight

If:

```
value[current] < value[next]
```

Then subtract current.
Else add current.

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<char, int> mp = {
            {'I',1},{'V',5},{'X',10},{'L',50},
            {'C',100},{'D',500},{'M',1000}
        };

        int result = 0;

        for(int i = 0; i < s.size(); i++) {
            if(i + 1 < s.size() && mp[s[i]] < mp[s[i+1]]) {
                result -= mp[s[i]];
            } else {
                result += mp[s[i]];
            }
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

Cleaner and more elegant.

---

# 🟢 3. Optimum Solution (Right-to-Left Traversal)

### 💡 Even Cleaner Approach

Traverse from right to left.

Maintain `prevValue`.

If current value < prevValue → subtract
Else → add

This avoids checking next index.

---

### 💻 Code (C++)

```cpp
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<char, int> mp = {
            {'I',1},{'V',5},{'X',10},{'L',50},
            {'C',100},{'D',500},{'M',1000}
        };

        int result = 0;
        int prevValue = 0;

        for(int i = s.size() - 1; i >= 0; i--) {
            int currentValue = mp[s[i]];

            if(currentValue < prevValue) {
                result -= currentValue;
            } else {
                result += currentValue;
            }

            prevValue = currentValue;
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

⭐ Most elegant and interview-friendly.

---

# 🔍 Dry Run (MCMXCIV)

Traverse from right:

```
V = 5  → +5
I = 1  → 1 < 5 → -1 → total = 4
C = 100 → +100 → 104
X = 10 → 10 < 100 → -10 → 94
M = 1000 → +1000 → 1094
C = 100 → 100 < 1000 → -100 → 994
M = 1000 → +1000 → 1994
```

---

# 🏆 Final Comparison

| Approach           | Time | Space | Recommended |
| ------------------ | ---- | ----- | ----------- |
| Special Pair Check | O(N) | O(1)  | ❌           |
| Compare Next       | O(N) | O(1)  | ✅           |
| Right-to-Left      | O(N) | O(1)  | ⭐ Best      |

---
