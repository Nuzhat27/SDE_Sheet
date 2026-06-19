# Number of Inversions
**LeetCode / GFG:** [Count Inversions](https://www.geeksforgeeks.org/problems/inversion-of-array-1587115620/1)  
**Difficulty:** 🔴 Hard  
**Submission:** ✅ Accepted — 119/119 test cases

---

## Problem Statement

Given an array `nums`, count the number of **inversions** — pairs `(i, j)` where `i < j` but `nums[i] > nums[j]`.

**Example:**
```
Input:  nums = [2, 4, 1, 3, 5]
Output: 3
Explanation: (2,1), (4,1), (4,3) are the inversion pairs.
```

---

## 🧠 Intuition

**Brute force** — check every pair `(i, j)` with `i < j` and count those where `nums[i] > nums[j]`. Time: O(n²).

**Optimal — Modified Merge Sort.** During the merge step of merge sort, when an element from the **right half** is placed before elements remaining in the **left half**, every one of those remaining left-half elements forms an inversion with it. Since the left half is already sorted, the count is exactly `mid - i + 1` — added in O(1).

This piggybacks inversion counting on top of the natural comparison work merge sort already does — no redundant work, no extra pass.

---

## ✅ Solution

```java
class Solution {
    public long numberOfInversions(int[] nums) {
        int n = nums.length;
        return mergeSort(nums, 0, n - 1);
    }

    public long mergeSort(int[] nums, int low, int high) {
        long cnt = 0;
        if (low < high) {
            int mid = (low + high) / 2;
            cnt += mergeSort(nums, low, mid);
            cnt += mergeSort(nums, mid + 1, high);
            cnt += merge(nums, low, mid, high);
        }
        return cnt;
    }

    public long merge(int[] nums, int low, int mid, int high) {
        long cnt = 0;
        int[] temp = new int[high - low + 1];
        int i = low, j = mid + 1, k = 0;

        while (i <= mid && j <= high) {
            if (nums[i] <= nums[j]) {
                temp[k++] = nums[i++];
            } else {
                // nums[i..mid] are all > nums[j] → (mid - i + 1) inversions
                cnt += (mid - i + 1);
                temp[k++] = nums[j++];
            }
        }

        while (i <= mid)  temp[k++] = nums[i++];
        while (j <= high) temp[k++] = nums[j++];

        for (int x = 0; x < temp.length; x++) {
            nums[low + x] = temp[x];
        }

        return cnt;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| **Time** | O(n log n) |
| **Space** | O(n) — temporary merge array |

---

## 💡 Key Takeaway

Merge sort doesn't just sort — it also **reveals order information** between the two halves at every merge step. Counting inversions is a natural byproduct: when a right-half element jumps ahead of left-half elements, the sorted left sub-array tells you exactly how many inversions that creates in O(1) per element, giving an overall O(n log n) solution.

---

[← Day 04](./README.md)
