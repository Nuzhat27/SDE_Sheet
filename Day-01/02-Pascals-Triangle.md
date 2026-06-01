# Pascal's Triangle — Find nCr

**LeetCode:** [119. Pascal's Triangle II](https://takeuforward.org/plus/dsa/problems/pascals-triangle-i?source=strivers-sde-sheet/)  
**Difficulty:** 🟢 Easy  
**Submission:** ✅ Accepted — 3474/3474 test cases

---

## Problem Statement

Given row index `n` and column index `r`, return the value at that position in Pascal's Triangle, which equals the binomial coefficient `C(n-1, r-1)`.

**Example:**
```
Input:  n = 5, r = 3
Output: 6        (C(4,2) = 6)
```

---

## 🧠 Intuition

Every element in Pascal's Triangle is a binomial coefficient `C(n, r)`.

Instead of computing full factorials (which overflow quickly), use the **multiplicative formula**:

```
C(n, r) = [n × (n-1) × ... × (n-r+1)] / r!
```

Which iteratively computes as:
```
for i = 0 to r-1:
    res = res * (n - i) / (i + 1)
```

**Dividing at each step** (not at the end) keeps intermediate values small and prevents integer overflow.

Two further optimizations:
- If `r > n - r`, use `r = n - r` since `C(n, r) = C(n, n-r)` — compute using the smaller side
- If `r == 1`, return `n` directly

---

## ✅ Solution

```java
class Solution {
    public int pascalTriangleI(int n, int r) {
        return findNCR(n - 1, r - 1);
    }

    public int findNCR(int n, int r) {
        if (r > n - r) r = n - r;   // use smaller r (symmetry of nCr)
        if (r == 1) return n;        // base case

        int res = 1;
        for (int i = 0; i < r; i++) {
            res = res * (n - i);     // multiply numerator term
            res = res / (i + 1);     // divide denominator term immediately
        }
        return res;
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| **Time** | O(r) |
| **Space** | O(1) |

---

## 💡 Key Takeaway

Compute `nCr` iteratively as `res = res * (n-i) / (i+1)`. Divide at each step to avoid overflow. Use symmetry `C(n,r) = C(n, n-r)` to always loop the minimum number of times.

---

[← Day 01](./README.md)
