# Day45 动态规划

● 70. 爬楼梯 （进阶）

● 322. 零钱兑换 

● 279.完全平方数 

## [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

```cpp
class Solution {
public:
    int climbStairs(int n) {
        if(n<=1) return 1;
        vector<int> dp(n+1, 0);
        dp[0] = 1, dp[1] = 1;
        for(int i = 2; i <= n; i ++ ) dp[i] = dp[i-1] + dp[i-2];
        return dp[n];
    }
};
```

## [322. 零钱兑换](https://leetcode.cn/problems/coin-change/description/)

给你一个整数数组 `coins` ，表示不同面额的硬币；以及一个整数 `amount` ，表示总金额。

计算并返回可以凑成总金额所需的 **最少的硬币个数** 。如果没有任何一种硬币组合能组成总金额，返回 `-1` 。

你可以认为每种硬币的数量是无限的。

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, INT_MAX);
        dp[0] = 0;
        for(int i = 0; i < coins.size(); i ++ ) {
            for(int j = coins[i]; j <= amount; j ++ ) {
                if(dp[j - coins[i]] != INT_MAX) {
                    dp[j] = min(dp[j], dp[j - coins[i]] + 1);
                }
            }
        }
        if(dp[amount] == INT_MAX) return -1;
        return dp[amount];
    }
};
```

