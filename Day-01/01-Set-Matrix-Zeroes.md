# Set Matrix Zeroes

**LeetCode:** [73. Set Matrix Zeroes]([https://leetcode.com/problems/set-matrix-zeroes/](https://takeuforward.org/plus/dsa/problems/set-matrix-zeroes?source=strivers-sde-sheet)  
**Difficulty:** 🟡 Medium  
**Submission:** ✅ Accepted — 200/200 test cases

---

## Problem Statement

Given an `m x n` integer matrix, if an element is `0`, set its entire row and column to `0`. Must be done **in-place**.

**Example:**
```
Input:  [[1,1,1],
          [1,0,1],
          [1,1,1]]

Output: [[1,0,1],
          [0,0,0],
          [1,0,1]]
```

---

## 🧠 Intuition

**Brute force** — use an extra `m x n` matrix to store which cells to zero. Space: O(m×n).  
**Better** — use two separate arrays of size `m` and `n` to mark which rows/cols need zeroing. Space: O(m+n).  
**Optimal** — use the **first row and first column of the matrix itself** as the marker arrays. Space: O(1).

The only conflict is `matrix[0][0]` — it would need to serve as a marker for both row 0 and column 0. We resolve this with a separate boolean `col0` dedicated to column 0, while `matrix[0][0]` handles row 0 only.

**Key rule:** Always update the interior cells first, then fix the first row and column last — otherwise you corrupt your own markers before reading them.

---

## ✅ Solution

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        boolean col0 = false;

        // Step 1: Mark first row and first column as indicators
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;           // mark row i
                    if (j == 0) col0 = true;    // column 0 flag
                    else matrix[0][j] = 0;      // mark column j
                }
            }
        }

        // Step 2: Zero out interior cells using markers
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }

        // Step 3: Zero out first row if matrix[0][0] == 0
        if (matrix[0][0] == 0) {
            for (int j = 0; j < n; j++) matrix[0][j] = 0;
        }

        // Step 4: Zero out first column if col0 == true
        if (col0) {
            for (int i = 0; i < m; i++) matrix[i][0] = 0;
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| **Time** | O(m × n) |
| **Space** | O(1) |

---

## 💡 Key Takeaway

Use the matrix's own first row/column as marker arrays to achieve O(1) space. Handle the `(0,0)` overlap with a separate `col0` boolean. Always process the interior before the edges.

---

[← Day 01](./README.md)
