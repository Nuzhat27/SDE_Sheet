# Best Time to Buy & Sell Stock

**LeetCode:** [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
**Difficulty:** 🟢 Easy
**Submission:** ✅ Accepted — 114/114 test cases

---

## Problem Statement

Given an array `arr` where `arr[i]` is the price of a stock on day `i`, find the **maximum profit** you can achieve by buying on one day and selling on a later day. Return `0` if no profit is possible.

**Example:**
```
Input:  arr = [7, 1, 5, 3, 6, 4]
Output: 5
Explanation: Buy on day 2 (price = 1), sell on day 5 (price = 6). Profit = 6 - 1 = 5.
```

---

## 🧠 Intuition

**Brute force** — try every pair (buy day, sell day) where buy < sell and track the max difference. Time: O(n²).

**Optimal** — one pass, two variables: `curPrice` (minimum price seen so far) and `maxProfit` (best profit seen so far).

At each price, two things happen simultaneously:
1. Update `curPrice` = min of current and previous minimum — this is the best possible buy day up to now.
2. Update `maxProfit` = max of current profit (`price - curPrice`) and the best profit so far.

We never need to look back because if a lower buy price appeared before the current sell day, `curPrice` already holds it.

---

## ✅ Solution

```java
class Solution {
    public int stockBuySell(int[] arr, int n) {
        int curPrice = arr[0], maxProfit = 0;
        for (int price : arr) {
            curPrice = Math.min(price, curPrice);
            maxProfit = Math.max(maxProfit, price - curPrice);
        }
        return maxProfit;
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

Track the running minimum as you scan — the best profit at any point is always `current price − lowest price seen so far`. No need for nested loops.

---

[← Back to Day 02](./README.md)
