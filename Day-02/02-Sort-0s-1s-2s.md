# Sort 0s 1s 2s

**LeetCode:** [75. Sort Colors](https://leetcode.com/problems/sort-colors/)
**Difficulty:** 🟡 Medium
**Submission:** ✅ Accepted — 117/117 test cases

---

## Problem Statement

Given an array `nums` containing only `0`, `1`, and `2`, sort it **in-place** so that all 0s come first, then 1s, then 2s — without using the built-in sort.

**Example:**
```
Input:  [2, 0, 2, 1, 1, 0]
Output: [0, 0, 1, 1, 2, 2]
```

---

## 🧠 Intuition

**Brute force** — count the frequency of 0s, 1s, and 2s, then overwrite the array. Two passes, O(n) time, O(1) space — works, but needs two traversals.

**Optimal (Dutch National Flag Algorithm)** — maintain three pointers:
- `low` — everything before `low` is 0
- `mid` — current element under inspection
- `high` — everything after `high` is 2

While `mid <= high`:
- `nums[mid] == 0` → swap with `low`, advance both `low` and `mid`
- `nums[mid] == 1` → it's in the right place, just advance `mid`
- `nums[mid] == 2` → swap with `high`, shrink `high` (don't advance `mid` — the swapped-in element is uninspected)

One single pass, no extra space.

---

## ✅ Solution

```java
class Solution {
    public void sortZeroOneTwo(int[] nums) {
        int n = nums.length;
        int low = 0, mid = 0, high = n - 1;
        while (mid <= high) {
            if (nums[mid] == 0) {
                int temp = nums[low];
                nums[low] = nums[mid];
                nums[mid] = temp;
                low++; mid++;
            } else if (nums[mid] == 1) {
                mid++;
            } else {
                int temp = nums[mid];
                nums[mid] = nums[high];
                nums[high] = temp;
                high--;
            }
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| **Time** | O(n) |
| **Space** | O(1) |

---

## 💡 Key Takeaway

Dutch National Flag: three pointers carve the array into four regions — sorted 0s, sorted 1s, unseen middle, sorted 2s — and shrink the unseen region to nothing in a single pass.

---

[← Back to Day 02](./README.md)
