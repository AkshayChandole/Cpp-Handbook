# [Remove All Adjacent Duplicates in String II](#remove-all-adjacent-duplicates-in-string-ii)

## 🟢 Remove All Adjacent Duplicates in String II

---

### 📌 Problem

Given a string `s` and an integer `k`, repeatedly remove **k adjacent equal characters** from the string.

Return the final string after all possible removals.

---

### 📌 Examples

```id="ex1"
Input: s = "abcd", k = 2
Output: "abcd"
Explanation: No duplicates of length 2.
```

```id="ex2"
Input: s = "deeedbbcccbdaa", k = 3
Output: "aa"
Explanation:
deeedbbcccbdaa
→ ddd removed → eedbbcccbdaa
→ eee removed → dbbcccbdaa
→ ccc removed → dbbdaa
→ bbb removed → daa
→ aaa removed → aa
```

```id="ex3"
Input: s = "pbbcggttciiippooaais", k = 2
Output: "ps"
```

---

# 🔴 1. Brute Force (Repeated Scan & Erase)

### 💡 Idea

* Scan string.
* If k consecutive characters found:

  * Remove them using erase.
* Repeat until no change.

---

### 💻 Code (C++ – Inefficient)

```cpp id="bf1"
class Solution {
public:
    string removeDuplicates(string s, int k) {
        bool changed = true;

        while(changed) {
            changed = false;
            for(int i = 0; i <= s.size() - k; i++) {
                string temp = s.substr(i, k);
                if(count(temp.begin(), temp.end(), temp[0]) == k) {
                    s.erase(i, k);
                    changed = true;
                    break;
                }
            }
        }

        return s;
    }
};
```

---

### ⏱ Time Complexity

O(N²) worst case

### 🗂 Space Complexity

O(1)

❌ Too slow due to repeated erase.

---

# 🟡 2. Better Solution (Stack of Characters)

### 💡 Idea

Use stack and count consecutive characters.

But if we only store characters, we must count manually → inefficient.

Better to store:

```id="pair"
pair<char, count>
```

---

# 🟢 3. Optimum Solution (Stack with Count)

### 💡 Key Insight

Maintain stack of:

```id="structure"
{character, frequency}
```

For each character:

* If same as top → increment count.
* If count == k → pop.
* Else push new pair.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    string removeDuplicates(string s, int k) {
        vector<pair<char, int>> st;

        for(char c : s) {
            if(!st.empty() && st.back().first == c) {
                st.back().second++;
                if(st.back().second == k)
                    st.pop_back();
            } else {
                st.push_back({c, 1});
            }
        }

        string result;

        for(auto& p : st) {
            result.append(p.second, p.first);
        }

        return result;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(N)

⭐ Most optimal solution.

---

# 🔍 Dry Run

Example:

```id="dry1"
s = "deeedbbcccbdaa", k = 3
```

Process:

| Character | Stack             |
| --------- | ----------------- |
| d         | (d,1)             |
| e         | (d,1),(e,1)       |
| e         | (d,1),(e,2)       |
| e         | pop (e,3)         |
| d         | (d,2)             |
| b         | (d,2),(b,1)       |
| b         | (d,2),(b,2)       |
| c         | (d,2),(b,2),(c,1) |
| c         | (d,2),(b,2),(c,2) |
| c         | pop (c,3)         |
| b         | pop (b,3)         |
| d         | pop (d,3)         |
| a         | (a,1)             |
| a         | (a,2)             |

Final = `"aa"`

---

# 🎯 Why Stack Works

Because:

* We only care about adjacent duplicates.
* Stack naturally handles local grouping.
* When a group is removed, new adjacent groups automatically merge.

---

# 🔥 Important Insight

This is similar to:

* Valid Parentheses (stack usage)
* Remove Adjacent Duplicates I
* String compression problems

---

# 🏆 Final Comparison

| Approach         | Time  | Space | Recommended |
| ---------------- | ----- | ----- | ----------- |
| Repeated Erase   | O(N²) | O(1)  | ❌           |
| Stack with Count | O(N)  | O(N)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* Stack pattern
* Frequency tracking
* Handling cascading removals

Related problems:

* Remove Adjacent Duplicates I
* Valid Parentheses
* Decode String
* Basic Calculator

---

