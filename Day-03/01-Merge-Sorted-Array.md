# Merge Sorted Array

**LeetCode:** [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)  
**Difficulty:** 🟢 Easy  
**Submission:** ✅ Accepted — 117/117 test cases

---

## Problem Statement
`nums1` has size `m + n`, with the first `m` elements being the actual sorted data and the remaining `n` slots set to `0` as placeholders. `nums2` has size `n` and is also sorted. Merge `nums2` into `nums1` so the result is one sorted array of size `m + n`, **in-place**.

**Example:**
```
Input:  nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
```

---

## 🧠 Intuition
**Brute force** — copy `nums2` into the empty slots of `nums1` and sort the whole array. Time: O((m+n) log(m+n)).

**Optimal — two pointers from the back.** Merging from the front would require shifting elements forward to make room. Merging from the **back** avoids that entirely: compare the largest remaining elements of each array and place the bigger one at the last open slot, working backward. Since `nums1` has empty space at the end, nothing gets overwritten before it's read.

---

## ✅ Solution
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int p0 = m + n - 1; // last slot to fill in nums1
        int p1 = m - 1;     // last valid element in nums1
        int p2 = n - 1;     // last element in nums2

        while (p1 >= 0 && p2 >= 0) {
            if (nums1[p1] >= nums2[p2]) {
                nums1[p0--] = nums1[p1--];
            } else {
                nums1[p0--] = nums2[p2--];
            }
        }

        // Copy any remaining elements from nums2 (nums1's leftovers are already in place)
        while (p2 >= 0) {
            nums1[p0--] = nums2[p2--];
        }
    }
}
```

---

## ⏱️ Complexity
| | Complexity |
|---|---|
| **Time** | O(m + n) |
| **Space** | O(1) |

---

## 💡 Key Takeaway
When merging into a buffer that has empty space at one end, filling from that end backward avoids overwriting unread data — no shifting and no extra array required.

---
[← Day 03](./README.md)
