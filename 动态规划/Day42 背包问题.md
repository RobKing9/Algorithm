# Day42 背包问题

- 确定dp数组以及下标的含义

对于背包问题，有一种写法， 是使用二维数组，即`dp[i][j]` 表示从下标为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少**。

- 确定递推公式

再回顾一下dp[i][j]的含义：从下标为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少。

那么可以有两个方向推出来dp[i][j]，

- **不放物品i**：由dp[i - 1][j]推出，即背包容量为j，里面不放物品i的最大价值，此时dp[i][j]就是dp[i - 1][j]。(其实就是当物品i的重量大于背包j的重量时，物品i无法放进背包中，所以被背包内的价值依然和前面相同。)
- **放物品i**：由dp[i - 1][j - weight[i]]推出，dp[i - 1][j - weight[i]] 为背包容量为j - weight[i]的时候不放物品i的最大价值，那么dp[i - 1][j - weight[i]] + value[i] （物品i的价值），就是背包放物品i得到的最大价值

所以递归公式： dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);



## [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/description/)

给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int size = nums.size();
        int sum = 0;
        for(int i = 0; i < size; i ++) sum += nums[i];
        int tmp = sum / 2;
        if(sum%2) return false;
        vector<vector<int>> dp(size + 1, vector<int>(sum + 10, 0));
        dp[0][0] = dp[0][nums[0]] = 1;

        for(int i = 1; i < size; i ++ ) {
            for(int j = 0; j <= tmp; j ++) {
                if(j >= nums[i])    dp[i][j] = dp[i - 1][j - nums[i]]; // 选
                dp[i][j] |= dp[i - 1][j];
            }
        }
        return dp[size-1][tmp];
    }
};
```

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int size = nums.size();
        int sum = 0;
        for(int i = 0; i < size; i ++) sum += nums[i];
        int tmp = sum / 2;
        if(sum%2) return false;
        // vector<vector<int>> dp(size + 1, vector<int>(sum + 10, 0));
        vector<int> dp(sum + 10, 0);
        dp[0] = dp[nums[0]] = 1;

        for(int i = 1; i < size; i ++ ) {
            for(int j = tmp; j >= nums[i]; j --) {
                dp[j] |= dp[j - nums[i]];
                // if(j >= nums[i])    dp[i][j] = dp[i - 1][j - nums[i]]; // 选
                // dp[i][j] |= dp[i - 1][j];
            }
        }
        return dp[tmp];
    }
};
```

