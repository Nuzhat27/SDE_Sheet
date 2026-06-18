# Maximum Subarray

**LeetCode:** [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
**Difficulty:** 🟡 Medium
**Submission:** ✅ Accepted — 121/121 test cases

---

## Problem Statement

Given an integer array `nums`, find the **subarray** with the largest sum, and return its sum.

**Example:**
```
Input:  nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
Output: 6
Explanation: [4, -1, 2, 1] has the largest sum = 6
```

---

## 🧠 Intuition

**Brute force** — try every possible subarray and track the max sum. Time: O(n²) or O(n³).

**Optimal (Kadane's Algorithm)** — maintain a running `sum`. At each element, add it to `sum`. If `sum` ever beats `maxSum`, update `maxSum`. If `sum` drops below 0, reset it to 0 — a negative prefix only drags future subarrays down, so we discard it and "start fresh" from the next element.

The key insight: we never need to look back. A subarray ending at index `i` with the best possible sum is either the element alone, or the best subarray ending at `i-1` plus `nums[i]`. If the running sum goes negative, the best subarray ending at `i` is just `nums[i+1]` onward.

---

## ✅ Solution

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int n = nums.length;
        long maxSum = Integer.MIN_VALUE, sum = 0;
        for (int i = 0; i < n; i++) {
            sum += nums[i];
            if (sum > maxSum) {
                maxSum = sum;
            }
            if (sum < 0) {
                sum = 0;
            }
        }
        return (int) maxSum;
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

Kadane's Algorithm: keep a running sum and reset it to 0 whenever it goes negative. A negative prefix never helps — dropping it can only improve the sum of whatever comes next.

---

[← Back to Day 02](./README.md)
