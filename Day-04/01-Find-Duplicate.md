# Find the Duplicate Number
**LeetCode:** [287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)  
**Difficulty:** 🟡 Medium  
**Submission:** ✅ Accepted — 462/462 test cases

---

## Problem Statement

Given an array `nums` of `n + 1` integers where each integer is in the range `[1, n]`, there is **exactly one duplicate number**. Find and return it — without modifying the array and using only O(1) extra space.

**Example:**
```
Input:  nums = [1,3,4,2,2]
Output: 2
```

---

## 🧠 Intuition

**Brute force** — sort the array and check adjacent elements, or use a HashSet to track seen values. Time: O(n log n) or O(n); Space: O(n) for the set approach.

**Optimal — Floyd's Cycle Detection (Tortoise & Hare).** Treat each value as a pointer to the next index: `nums[i]` points to index `nums[i]`. Because one value appears twice, two indices point to the same next node — creating a cycle. The entry point of that cycle is the duplicate.

- **Phase 1:** Move `slow` one step and `fast` two steps at a time until they meet inside the cycle.
- **Phase 2:** Reset one pointer to `nums[0]`. Move both one step at a time — they meet at the cycle entrance, which is the duplicate.

This is the same algorithm used to detect cycles in a linked list, applied to an implicit graph formed by the array values.

---

## ✅ Solution

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0];
        int fast = nums[0];

        // Phase 1: Find intersection point inside the cycle
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);

        // Phase 2: Find the entry point of the cycle (the duplicate)
        slow = nums[0];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }

        return slow;
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

When an array of size `n + 1` holds values in `[1, n]`, it implicitly encodes a linked list with a cycle — and the duplicate is the node where two edges converge. Floyd's algorithm finds that node in linear time with no extra memory.

---

[← Day 04](./README.md)
