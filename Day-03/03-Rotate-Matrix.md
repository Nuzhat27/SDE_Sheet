# Rotate Matrix (90° CW)
**LeetCode:** [48. Rotate Image](https://leetcode.com/problems/rotate-image/)  
**Difficulty:** 🟡 Medium  
**Submission:** ✅ Accepted — 112/112 test cases

---

## Problem Statement

Given an `n x n` 2D matrix representing an image, rotate the matrix **90 degrees clockwise**, **in-place**.

**Example:**
```
Input:
[[1,2,3],
 [4,5,6],
 [7,8,9]]

Output:
[[7,4,1],
 [8,5,2],
 [9,6,3]]
```

---

## 🧠 Intuition

**Brute force** — allocate a new `n x n` matrix and place each element at its rotated position: `result[j][n-1-i] = matrix[i][j]`. Time: O(n²), Space: O(n²).

**Optimal — Transpose + Reverse Rows.** A 90° CW rotation is mathematically equivalent to two simpler operations done in sequence:
1. **Transpose** the matrix (reflect across the main diagonal): swap `matrix[i][j]` with `matrix[j][i]` for all `j > i`.
2. **Reverse each row**: flip the elements left-to-right in every row.

This works entirely in-place using only a single temp variable — no extra array needed.

---

## ✅ Solution

```java
class Solution {
    public void rotateMatrix(int[][] matrix) {
        int i, j, n, temp;
        n = matrix.length;

        // Step 1: Transpose (swap across the main diagonal)
        for (i = 0; i < n; i++) {
            for (j = i + 1; j < n; j++) {
                temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        // Step 2: Reverse each row
        for (i = 0; i < n; i++) {
            int left = 0, right = n - 1;
            while (left < right) {
                temp = matrix[i][left];
                matrix[i][left] = matrix[i][right];
                matrix[i][right] = temp;
                left++;
                right--;
            }
        }
    }
}
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| **Time** | O(n²) |
| **Space** | O(1) |

---

## 💡 Key Takeaway

A 90° CW rotation = **Transpose + Reverse Rows**. Recognising this decomposition turns what seems like a complex spatial transformation into two simple, well-understood array operations — both doable in-place with no extra memory.

---

[← Day 03](./README.md)
