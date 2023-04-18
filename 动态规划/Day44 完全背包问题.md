# Day44 完全背包问题

● 完全背包

● 518. 零钱兑换 II 

● 377. 组合总和 Ⅳ  

## [完全背包问题](https://www.acwing.com/problem/content/3/)

```cpp
#include <bits/stdc++.h>
using namespace std;

const int N = 1010;
int weight[N], value[N];
int dp[N];

int main () {
    int n, bag;
    cin >> n >> bag;
    for(int i = 0; i < n; i ++ ) cin >> weight[i] >> value[i];
    
    // i从0开始 表示选第0件物品
    for(int i = 0; i < n; i ++ ) {
        // 从小到大 可以使得物品被多次选择
        for(int j = weight[i]; j <= bag; j ++ ) {
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
    
    cout << dp[bag] << endl;
    
    return 0;
    
}	
```

## [518. 零钱兑换 II](https://leetcode.cn/problems/coin-change-ii/)

给你一个整数数组 `coins` 表示不同面额的硬币，另给一个整数 `amount` 表示总金额。

请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回 `0` 。

假设每一种面额的硬币有无限个。 

题目数据保证结果符合 32 位带符号整数。

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount + 1, 0);
        dp[0] = 1;
        for(int i = 0; i < coins.size(); i ++ ) {
            for(int j = coins[i]; j <= amount; j ++ ) {
                dp[j] += dp[j - coins[i]];
            }
        }
        return dp[amount];
    }
};
```

