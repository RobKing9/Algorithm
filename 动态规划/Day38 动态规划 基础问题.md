# Day38 动态规划 基础问题

● 理论基础 

● 509. 斐波那契数 

● 70. 爬楼梯 

● 746. 使用最小花费爬楼梯 

## 理论基础

**对于动态规划问题，拆解为如下五步曲**

1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组

**找问题的最好方式就是把dp数组打印出来，看看究竟是不是按照自己思路推导的！**

**做动规的题目，写代码之前一定要把状态转移在dp数组的上具体情况模拟一遍，心中有数，确定最后推出的是想要的结果**。



## [509. 斐波那契数](https://leetcode.cn/problems/fibonacci-number/description/)

```cpp
class Solution {
public:
    int fib(int n) {
        if(n <= 1) return n;
        vector<int> dp(n+1, 0);
        dp[0] = 0, dp[1] = 1, dp[2] = 1;
        for(int i = 2; i <= n; i ++ ) dp[i] = dp[i-1] + dp[i-2];

        return dp[n];
    }
};
```

## [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/description/)

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

## [746. 使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/description/)

给你一个整数数组 `cost` ，其中 `cost[i]` 是从楼梯第 `i` 个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。

你可以选择从下标为 `0` 或下标为 `1` 的台阶开始爬楼梯。

请你计算并返回达到楼梯顶部的最低花费。

```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        // dp[i] 表示达到i的最少花费
        vector<int> dp(cost.size()+1, 0);
        // 到达0 和 到达1 都不需要花费
        dp[0] = 0, dp[1] = 0;
        for(int i = 2; i <= cost.size(); i ++) 
            dp[i] = min(dp[i-1] + cost[i-1],dp[i-2] + cost[i-2]);

        return dp[cost.size()];
    }
};
```

这里注意的细节就是初始化的时候，dp[0] 和 dp[1] 都是0，因为到达这两个位置是不需要花费的

到达的那个位置可以由前一个过来，也可以从前两个过来