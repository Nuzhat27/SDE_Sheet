# Merge Intervals
**LeetCode:** [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)  
**Difficulty:** 🟡 Medium  
**Submission:** ✅ Accepted — 138/138 test cases

---

## Problem Statement

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals and return an array of the non-overlapping intervals that cover all the intervals in the input.

**Example:**
```
Input:  intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: [1,3] and [2,6] overlap → merged as [1,6].
```

---

## 🧠 Intuition

**Brute force** — for every interval, check all others to see if they overlap and merge them iteratively. Time: O(n²).

**Optimal — Sort + Greedy Merge.** Once intervals are sorted by their start time, any overlapping intervals are guaranteed to be adjacent. This means a single left-to-right pass is enough:
- Keep track of a `current` interval.
- For each `next` interval, if `next.start <= current.end`, they overlap — extend `current.end` to `max(current.end, next.end)`.
- Otherwise, `current` is complete — add it to the result and set `current = next`.

Sorting ensures we never miss an overlap, and the greedy merge handles both partial and complete overlaps in one shot.

---

## ✅ Solution

```java
class Solution {
    public List<List<Integer>> mergeOverlap(List<List<Integer>> intervals) {
        if (intervals.size() == 0) return new ArrayList<>();

        // Sort by start time
        intervals.sort((a, b) -> a.get(0) - b.get(0));

        List<List<Integer>> result = new ArrayList<>();
        List<Integer> current = intervals.get(0);

        for (int i = 1; i < intervals.size(); i++) {
            List<Integer> next = intervals.get(i);

            if (next.get(0) <= current.get(1)) {
                // Overlapping — extend current interval's end if needed
                current.set(1, Math.max(current.get(1), next.get(1)));
            } else {
                // No overlap — finalize current and move on
                result.add(current);
                current = next;
            }
        }

        result.add(current); // Add the last interval
        return result;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| **Time** | O(n log n) — dominated by sorting |
| **Space** | O(n) — for the output list |

---

## 💡 Key Takeaway

**Sorting brings order to chaos.** Once intervals are sorted by start time, overlapping ones are always neighbours — turning a potentially O(n²) comparison problem into a clean O(n) greedy pass. Always ask: *"what property would make this trivially solvable?"* and sort to achieve it.

---

[← Day 03](./README.md)
