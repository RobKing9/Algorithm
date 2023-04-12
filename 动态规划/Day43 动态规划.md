# Day43 动态规划

● 1049. 最后一块石头的重量 II 

● 494. 目标和 

 ● 474.一和零  

## [1049. 最后一块石头的重量 II](https://leetcode.cn/problems/last-stone-weight-ii/description/)

有一堆石头，用整数数组 `stones` 表示。其中 `stones[i]` 表示第 `i` 块石头的重量。

每一回合，从中选出**任意两块石头**，然后将它们一起粉碎。假设石头的重量分别为 `x` 和 `y`，且 `x <= y`。那么粉碎的可能结果如下：

- 如果 `x == y`，那么两块石头都会被完全粉碎；
- 如果 `x != y`，那么重量为 `x` 的石头将会完全粉碎，而重量为 `y` 的石头新重量为 `y-x`。

最后，**最多只会剩下一块** 石头。返回此石头 **最小的可能重量** 。如果没有石头剩下，就返回 `0`。

```cpp
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int sum = 0;
        for(int i = 0; i < stones.size(); i ++ ) sum += stones[i];
        int tmp = sum / 2;  // 向下取整
        vector<int> dp(sum + 10, 0);    // dp[i] 表示 和为i 的最大石子重量
        for(int i = 0; i < stones.size(); i ++ ){
            for(int j = tmp; j >= stones[i]; j --) {
                dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);  // 选择这块石头 或者 不选
            }
        }
        // 碰撞 得到剩下的
        return sum - dp[tmp] - dp[tmp];
    }
};
```



## [494. 目标和](https://leetcode.cn/problems/target-sum/description/)

给你一个整数数组 `nums` 和一个整数 `target` 。

向数组中的每个整数前添加 `'+'` 或 `'-'` ，然后串联起所有整数，可以构造一个 **表达式** ：

- 例如，`nums = [2, 1]` ，可以在 `2` 之前添加 `'+'` ，在 `1` 之前添加 `'-'` ，然后串联起来得到表达式 `"+2-1"` 。

返回可以通过上述方法构造的、运算结果等于 `target` 的不同 **表达式** 的数目。

```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        // 加法总和为 x，减法总和为 sum - x；x - (sum-x) = target => x = (target + sum) / 2
        int sum = 0;
        for(int i = 0; i < nums.size(); i ++ ) sum += nums[i];
        // 没有方案
        if((target + sum) % 2) return 0;
        if(abs(target) > sum) return 0;
        // 加法总和相当于 背包的容量
        int bag = (target + sum) / 2;
        vector<int> dp(bag + 1, 0);
        dp[0] = 1;
        for(int i = 0; i < nums.size(); i ++ ) {
            for(int j = bag; j >= nums[i]; j -- ) {
                dp[j] += dp[j - nums[i]]; 
            }
        }
        return dp[bag];
    }
};
```

有点没看懂