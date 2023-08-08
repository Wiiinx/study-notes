---
title: "518-Coin Change II"
date: 2023-08-08
Difficulty: Medium
Skills: Dynamic Programming
---
# 518. Coin Change II


# Description

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the number of combinations that make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

similar 

[**322. Coin Change**](https://www.notion.so/322-Coin-Change-2ae7329b46454933836ae0950229be3a?pvs=21)

# Sample Inputs

**Example 1:**

```python
Input: amount = 5, coins = [1,2,5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

**Example 2:**

```python
Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.
```

**Example 3:**

```python
Input: amount = 10, coins = [10]
Output: 1
```

# Solutions

[Neetcode Solution](https://www.youtube.com/watch?v=Mjy4hd2xgrs)

![Untitled](518%20Coin%20Change%20II%20f81b8f44f67f412ea9835b9353b80139/Untitled.png)

### Python: DFS solution

```python
class Solution(object):
    def change(self, amount, coins):
        """
        :type amount: int
        :type coins: List[int]
        :rtype: int
        """
        cache = {}
        def dfs(i, a):
            if a == amount: 
                return 1
            if a > amount: 
                return 0
            if i == len(coins): 
                return 0
            if (i, a) in cache: 
                return cache[(i, a)]
            # chosing coint at i, new is a + coint at index[i], second: skip coint at index i, but incrementing index
            cache[(i, a)] = dfs(i, a+coins[i]) + dfs(i + 1, a)
            return cache[(i, a)]
        
        return dfs(0, 0)
        
```

### Python: DP solution1

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [[0] * (len(coins)+1) for i in range(amount + 1)]
        dp[0] = [1] * (len(coins) + 1)

        for a in range(1, amount + 1):
            for i in range(len(coins)-1, -1, -1):
                dp[a][i] = dp[a][i + 1] # case1: skip coin of index i, passing i+1
                
                #case2: use coin at index i, use "-", a is target
                if a - coins[i] >= 0: 
                    dp[a][i] += dp[a - coins[i]][i]
        return dp[amount][0]
```

### Python: DP solution2: O(n) memory!!!!

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        # memo = O(n)
        dp = [0] * (amount + 1)
        dp[0] = 1

        # order iterating through the coins
        for i in range(len(coins) - 1, -1, -1):
            nextDP = [0] * (amount + 1)
            nextDP[0] = 1

            # inner loop interating through the amount
            for a in range(1, amount+1):
                nextDP[a] = dp[a]
                if a - coins[i] >= 0:
                    nextDP[a] += nextDP[a - coins[i]]
                    
            # reassign dp, only 2 row of memory
            dp = nextDP
        return dp[amount]
```
