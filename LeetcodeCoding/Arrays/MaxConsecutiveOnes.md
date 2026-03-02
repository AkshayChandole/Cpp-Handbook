# [Max Consecutive Ones](#max-consecutive-ones)

## 🟢 Max Consecutive Ones

---

### 📌 Problem

Given a binary array `nums`, return the **maximum number of consecutive `1`s** in the array.

---

### 📌 Examples

```id="ex1"
Input: nums = [1,1,0,1,1,1]
Output: 3
Explanation: The first two digits or the last three digits are consecutive 1s.
The maximum number is 3.
```

```id="ex2"
Input: nums = [1,0,1,1,0,1]
Output: 2
```

---

# 🔴 1. Brute Force (Check Every Subarray)

### 💡 Idea

Generate all subarrays and check if they contain only 1s.

Track maximum length.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int n = nums.size();
        int maxLen = 0;

        for(int i = 0; i < n; i++) {
            for(int j = i; j < n; j++) {
                bool allOnes = true;

                for(int k = i; k <= j; k++) {
                    if(nums[k] == 0) {
                        allOnes = false;
                        break;
                    }
                }

                if(allOnes)
                    maxLen = max(maxLen, j - i + 1);
            }
        }

        return maxLen;
    }
};
```

---

### ⏱ Time Complexity

O(N³)

### 🗂 Space Complexity

O(1)

❌ Extremely inefficient.

---

# 🟡 2. Better Solution (Two Nested Loops)

### 💡 Idea

Fix starting index and count consecutive 1s until a 0 appears.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int n = nums.size();
        int maxLen = 0;

        for(int i = 0; i < n; i++) {
            int count = 0;

            for(int j = i; j < n; j++) {
                if(nums[j] == 1)
                    count++;
                else
                    break;

                maxLen = max(maxLen, count);
            }
        }

        return maxLen;
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(1)

Still not optimal.

---

# 🟢 3. Optimum Solution (Single Pass)

### 💡 Key Insight

We only need:

* A running count of consecutive 1s.
* Reset count when we see 0.
* Track maximum.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int maxLen = 0;
        int currentCount = 0;

        for(int num : nums) {
            if(num == 1) {
                currentCount++;
                maxLen = max(maxLen, currentCount);
            } else {
                currentCount = 0;
            }
        }

        return maxLen;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

⭐ Most expected interview solution.

---

# 🔍 Dry Run

Input:

```id="dry1"
[1,1,0,1,1,1]
```

Step-by-step:

| Element | Current Count | Max |
| ------- | ------------- | --- |
| 1       | 1             | 1   |
| 1       | 2             | 2   |
| 0       | 0             | 2   |
| 1       | 1             | 2   |
| 1       | 2             | 2   |
| 1       | 3             | 3   |

Final Answer = 3

---

# 🎯 Why This Works

We don’t need to check all subarrays.

We only care about continuous streaks of 1s.

Whenever a 0 appears:

* The streak breaks.
* Reset counter.

---

# 🏆 Final Comparison

| Approach    | Time  | Space | Recommended |
| ----------- | ----- | ----- | ----------- |
| Triple Loop | O(N³) | O(1)  | ❌           |
| Double Loop | O(N²) | O(1)  | ❌           |
| Single Pass | O(N)  | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* Running window technique
* Tracking streaks
* Resetting logic on condition break

Follow-up variations:

* Max Consecutive Ones II (flip one zero)
* Max Consecutive Ones III (flip k zeros → sliding window)
* Longest Subarray with at most K zeros

---
