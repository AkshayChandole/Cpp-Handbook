# [Meeting Rooms II](#meeting-rooms-ii)

## 🟢 Meeting Rooms II

---

### 📌 Problem

Given an array of meeting time intervals:

```cpp
intervals[i] = [start_i, end_i]
```

Return the **minimum number of conference rooms required**.

---

### 📌 Example

```id="ex1"
Input: intervals = [[0,30],[5,10],[15,20]]
Output: 2
Explanation:
[0,30] overlaps with both [5,10] and [15,20]
```

```id="ex2"
Input: intervals = [[7,10],[2,4]]
Output: 1
```

---

# 🔴 1. Brute Force (Check Overlap for Each Meeting)

### 💡 Idea

For each meeting:

* Count how many meetings overlap with it.
* Take maximum overlap.

---

### 💻 Code (C++)

```cpp id="bf1"
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        int n = intervals.size();
        int maxRooms = 0;

        for(int i = 0; i < n; i++) {
            int rooms = 1;
            for(int j = 0; j < n; j++) {
                if(i != j &&
                   intervals[j][0] < intervals[i][1] &&
                   intervals[j][1] > intervals[i][0]) {
                    rooms++;
                }
            }
            maxRooms = max(maxRooms, rooms);
        }

        return maxRooms;
    }
};
```

---

### ⏱ Time Complexity

O(N²)

### 🗂 Space Complexity

O(1)

❌ Too slow.

---

# 🟡 2. Better Solution (Min Heap / Priority Queue)

### 💡 Key Idea

1. Sort intervals by start time.
2. Use a **min-heap** to track end times.
3. For each meeting:

   * If earliest ending meeting ends before current starts → reuse room.
   * Else → need new room.

---

### 💻 Code (C++)

```cpp id="better1"
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        if(intervals.empty()) return 0;

        sort(intervals.begin(), intervals.end());

        priority_queue<int, vector<int>, greater<int>> minHeap;

        minHeap.push(intervals[0][1]);

        for(int i = 1; i < intervals.size(); i++) {
            if(intervals[i][0] >= minHeap.top())
                minHeap.pop();

            minHeap.push(intervals[i][1]);
        }

        return minHeap.size();
    }
};
```

---

### ⏱ Time Complexity

O(N log N)

* Sorting → O(N log N)
* Heap operations → O(N log N)

### 🗂 Space Complexity

O(N)

✅ Very common solution.

---

# 🟢 3. Optimum Solution (Two Pointer – Sweep Line)

### 💡 Even Cleaner Idea

Separate:

* All start times
* All end times

Sort both arrays.

Use two pointers:

* If next start < next end → need new room.
* Else → room freed.

---

### 💻 Code (C++)

```cpp id="opt1"
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        int n = intervals.size();

        vector<int> start(n), end(n);

        for(int i = 0; i < n; i++) {
            start[i] = intervals[i][0];
            end[i] = intervals[i][1];
        }

        sort(start.begin(), start.end());
        sort(end.begin(), end.end());

        int rooms = 0, maxRooms = 0;
        int i = 0, j = 0;

        while(i < n) {
            if(start[i] < end[j]) {
                rooms++;
                i++;
            } else {
                rooms--;
                j++;
            }
            maxRooms = max(maxRooms, rooms);
        }

        return maxRooms;
    }
};
```

---

### ⏱ Time Complexity

O(N log N)

### 🗂 Space Complexity

O(N)

⭐ Most optimal and clean.

---

# 🔍 Why Two-Pointer Works

Example:

```id="dry1"
intervals = [[0,30],[5,10],[15,20]]
```

After sorting:

```id="dry2"
start = [0,5,15]
end   = [10,20,30]
```

Process:

| start[i]     | end[j] | rooms |
| ------------ | ------ | ----- |
| 0 < 10 → +1  |        |       |
| 5 < 10 → +1  |        |       |
| 15 ≥ 10 → -1 |        |       |
| 15 < 20 → +1 |        |       |

Max rooms = 2

---

# 🎯 Intuition

Think of it as timeline events:

* Start → +1 room
* End → -1 room

We just count maximum active meetings at any time.

---

# 🏆 Final Comparison

| Approach    | Time       | Space | Recommended |
| ----------- | ---------- | ----- | ----------- |
| Brute Force | O(N²)      | O(1)  | ❌           |
| Min Heap    | O(N log N) | O(N)  | ✅           |
| Sweep Line  | O(N log N) | O(N)  | ⭐ Best      |

---

# 🎯 Interview Insight

This is a classic **interval overlap** problem.

Related problems:

* Meeting Rooms I
* Merge Intervals
* Insert Interval
* Number of Airplanes in the Sky
* Car Pooling

---

