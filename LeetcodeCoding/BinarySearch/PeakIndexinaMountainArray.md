# [Peak Index in a Mountain Array](#peak-index-in-a-mountain-array)

## 🟢 Peak Index in a Mountain Array

---

### 📌 Problem

An array `arr` is a **mountain array** if:

* `arr.length >= 3`
* There exists some index `i` such that:

  * `arr[0] < arr[1] < ... < arr[i]`
  * `arr[i] > arr[i+1] > ... > arr[n-1]`

Return the index `i` of the peak element.

---

### 📌 Examples

```id="ex1"
Input: arr = [0,1,0]
Output: 1
```

```id="ex2"
Input: arr = [0,2,1,0]
Output: 1
```

```id="ex3"
Input: arr = [0,10,5,2]
Output: 1
```

---

# 🔴 1. Brute Force (Linear Scan for Maximum)

### 💡 Idea

Since peak is the maximum element:

* Traverse entire array.
* Return index of maximum.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {
        int maxIndex = 0;

        for(int i = 1; i < arr.size(); i++) {
            if(arr[i] > arr[maxIndex])
                maxIndex = i;
        }

        return maxIndex;
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

Works, but not optimal.

---

# 🟡 2. Better Solution (Linear Check for Peak Condition)

### 💡 Idea

Find index where:

```
arr[i] > arr[i-1] and arr[i] > arr[i+1]
```

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {
        for(int i = 1; i < arr.size() - 1; i++) {
            if(arr[i] > arr[i - 1] && arr[i] > arr[i + 1])
                return i;
        }
        return -1; // Should not happen in valid mountain array
    }
};
```

---

### ⏱ Time Complexity

O(N)

### 🗂 Space Complexity

O(1)

Still linear.

---

# 🟢 3. Optimum Solution (Binary Search)

### 💡 Key Insight

Mountain array has:

* Strictly increasing part
* Strictly decreasing part

So peak is where slope changes from positive to negative.

We can use **Binary Search**.

---

### 🔹 Logic

At `mid`:

* If `arr[mid] < arr[mid + 1]`
  → We are in increasing slope
  → Peak lies on right
  → `low = mid + 1`

* Else
  → We are in decreasing slope
  → Peak lies at mid or left
  → `high = mid`

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {
        int low = 0;
        int high = arr.size() - 1;

        while(low < high) {
            int mid = low + (high - low) / 2;

            if(arr[mid] < arr[mid + 1])
                low = mid + 1;
            else
                high = mid;
        }

        return low;
    }
};
```

---

### ⏱ Time Complexity

O(log N)

### 🗂 Space Complexity

O(1)

⭐ Most expected interview solution.

---

# 🔍 Dry Run

Example:

```id="dry1"
arr = [0,2,5,3,1]
```

Initial:

```id="dry2"
low = 0
high = 4
```

Step 1:

```id="dry3"
mid = 2
arr[2] = 5
arr[3] = 3

5 > 3 → decreasing slope
high = 2
```

Step 2:

```id="dry4"
low = 0
high = 2
mid = 1
arr[1] = 2
arr[2] = 5

2 < 5 → increasing slope
low = 2
```

Now:

```id="dry5"
low = high = 2
```

Peak index = 2

---

# 🎯 Why Binary Search Works

Because:

* The array increases then decreases.
* The slope direction tells which side the peak lies.
* This is similar to "Find Peak Element".

---

# 🏆 Final Comparison

| Approach         | Time     | Space | Recommended |
| ---------------- | -------- | ----- | ----------- |
| Linear Max       | O(N)     | O(1)  | ❌           |
| Linear Condition | O(N)     | O(1)  | ❌           |
| Binary Search    | O(log N) | O(1)  | ⭐ Best      |

---

# 🎯 Interview Insight

This problem teaches:

* Modified binary search
* Using slope direction
* Peak-finding pattern

Related problems:

* Find Peak Element
* Search in Rotated Sorted Array
* Find Minimum in Rotated Sorted Array
* Mountain Array Search (Hard)

---

