# Find Missing & Repeating Numbers
**LeetCode / GFG:** [Find Missing and Repeating](https://www.geeksforgeeks.org/problems/find-missing-and-repeating2512/1)  
**Difficulty:** 🟡 Medium  
**Submission:** ✅ Accepted — 116/116 test cases

---

## Problem Statement

Given an array of size `n` that should contain every integer from `1` to `n` exactly once, one number appears **twice** (repeating) and one number is **missing**. Find both.

**Example:**
```
Input:  nums = [3, 1, 3]
Output: [3, 2]   // repeating = 3, missing = 2
```

---

## 🧠 Intuition

**Brute force** — use a frequency array of size `n + 1`; the entry with count `2` is repeating, the entry with count `0` is missing. Time: O(n), Space: O(n).

**Optimal — Math (Sum + Sum of Squares).** Use two equations derived from the difference between the actual array and the ideal `[1…n]`:

Let **A** = repeating number, **B** = missing number.

- `S1 = sum(nums) - n*(n+1)/2` → equals `A - B`
- `S2 = sumOfSquares(nums) - n*(n+1)*(2n+1)/6` → equals `A² - B²` = `(A-B)(A+B)`

Dividing S2 by S1 gives `A + B`. Combined with `A - B = S1`, solving the two linear equations yields A and B directly — no extra array, no sorting.

> ⚠️ Use `long` for all intermediate calculations to avoid integer overflow on large inputs.

---

## ✅ Solution

```java
class Solution {
    public int[] findMissingRepeatingNumbers(int[] nums) {
        long s1 = 0, s2 = 0;
        long n = nums.length;

        for (int num : nums) {
            s1 += num;
            s2 += (long) num * (long) num;
        }

        // Expected sums for [1..n]
        long sn1 = (n * (n + 1)) / 2;
        long sn2 = (n * (n + 1) * (2 * n + 1)) / 6;

        // diffAB = A - B,  then derive A + B
        long diffAB = s1 - sn1;
        long sumAB  = (s2 - sn2) / diffAB;

        long A = (diffAB + sumAB) / 2;  // repeating
        long B = sumAB - A;             // missing

        return new int[]{ (int) A, (int) B };
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

Two unknowns need two equations. The sum formula gives `A - B`; the sum-of-squares formula gives `A² - B²`, which factors into `(A - B)(A + B)` — handing you `A + B` for free. Simple algebra then solves for both values in a single pass with no extra memory.

---

[← Day 04](./README.md)
