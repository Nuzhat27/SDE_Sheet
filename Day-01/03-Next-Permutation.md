# Next Permutation

**LeetCode:** [31. Next Permutation](https://takeuforward.org/plus/dsa/problems/next-permutation?source=strivers-sde-sheet/)  
**Difficulty:** 🟡 Medium  
**Submission:** ✅ Accepted — 315/315 test cases

---

## Problem Statement

Given an array of integers, rearrange it into the **next lexicographically greater permutation**. If no greater permutation exists (array is fully descending), rearrange to the **smallest possible order** (ascending). Must be done in-place.

**Example:**
```
Input:  [1, 2, 3]   →   Output: [1, 3, 2]
Input:  [3, 2, 1]   →   Output: [1, 2, 3]
Input:  [1, 1, 5]   →   Output: [1, 5, 1]
```

---

## 🧠 Intuition

Think about how you'd find the next number manually in a sequence like `1→2→3→4→...`.

From the right end, the suffix is always in descending order (that's why no greater permutation can be formed from it). We need to find the **first element from the right that breaks this descent** — this is the **pivot** (the dip).

Once found, swap the pivot with the **smallest element to its right that is still larger than it** (rightmost such element). This makes the number slightly larger. Then **reverse the entire suffix** after the pivot to get the smallest possible arrangement there.

**Steps:**
1. Scan right to left — find index `ind` where `nums[ind] < nums[ind+1]` (the pivot/dip)
2. If `ind == -1` — array is fully descending, reverse the whole array and return
3. Scan right to left again — find the rightmost element greater than `nums[ind]`, swap them
4. Reverse the suffix from `ind+1` to end

**Why reverse the suffix?**  
After the swap, the suffix is still in descending order. Reversing it gives the smallest possible arrangement — which ensures we get the very next permutation, not a larger jump.

---

## ✅ Solution

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int n = nums.length;
        int ind = -1;

        // Step 1: Find the pivot — first dip from the right
        for (int i = n - 2; i >= 0; i--) {
            if (nums[i] < nums[i + 1]) {
                ind = i;
                break;
            }
        }

        // Step 2: Fully descending — reverse entire array
        if (ind == -1) {
            reverse(nums, 0, n - 1);
            return;
        }

        // Step 3: Find rightmost element greater than nums[ind] and swap
        for (int i = n - 1; i > ind; i--) {
            if (nums[i] > nums[ind]) {
                int temp = nums[i];
                nums[i] = nums[ind];
                nums[ind] = temp;
                break;
            }
        }

        // Step 4: Reverse the suffix after ind
        reverse(nums, ind + 1, n - 1);
    }

    private void reverse(int[] nums, int l, int r) {
        while (l < r) {
            int temp = nums[l];
            nums[l] = nums[r];
            nums[r] = temp;
            l++;
            r--;
        }
    }
}
```

---

## 🔍 Dry Run

```
Input: [2, 1, 5, 4, 3, 0, 0]

Step 1 — find pivot:
  Scan right to left: 0 < 0? No. 3 < 0? No. 4 < 3? No. 5 < 4? No. 1 < 5? YES → ind = 1

Step 2 — not -1, continue

Step 3 — find rightmost element > nums[1]=1:
  Scan from right: 0 > 1? No. 0 > 1? No. 3 > 1? YES → swap nums[1] and nums[4]
  Array: [2, 3, 5, 4, 1, 0, 0]

Step 4 — reverse suffix from ind+1=2 to end:
  [2, 3, 0, 0, 1, 4, 5]

Output: [2, 3, 0, 0, 1, 4, 5] ✓
```

---

## ⏱️ Complexity

| | Complexity |
|---|---|
| **Time** | O(n) — at most 3 linear passes |
| **Space** | O(1) — in-place |

---

## 💡 Key Takeaway

Find the rightmost dip (pivot), swap with the next greater from the right, reverse the suffix. Three simple passes give the exact next permutation in O(n) time and O(1) space.

---

[← Day 01](./README.md)
