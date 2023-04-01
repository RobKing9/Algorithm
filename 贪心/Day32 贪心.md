# Day32 贪心

● 122.买卖股票的最佳时机II 

● 55. 跳跃游戏 

● 45.跳跃游戏II 

## [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/)

给你一个整数数组 `prices` ，其中 `prices[i]` 表示某支股票第 `i` 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 **最多** 只能持有 **一股** 股票。你也可以先购买，然后在 **同一天** 出售。

返回 *你能获得的 **最大** 利润* 。

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res = 0;
        for(int i = 1; i < prices.size(); i ++ ){
            int tmp = prices[i] - prices[i-1];
            if(tmp > 0) res += tmp;
        }
        return res;
    }
};
```

**收集正利润的区间，就是股票买卖的区间，而我们只需要关注最终利润，不需要记录区间**。

那么只收集正利润就是贪心所贪的地方！

**局部最优：收集每天的正利润，全局最优：求得最大利润**。



## [55. 跳跃游戏](https://leetcode.cn/problems/jump-game/description/)

给定一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标。

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        if(nums.size() <= 1) return true;
        // 看覆盖范围是否可以 覆盖最后一个下标
        int cover = 0;
        for(int i = 0; i <= cover; i ++ ) {
            cover = max(cover, nums[i] + i);    // 更新最大覆盖范围
            if(cover >= nums.size() - 1) return true;   // 可以覆盖最后一个元素了
        }
        return false;
    }   
};
```

其实跳几步无所谓，关键在于可跳的覆盖范围！

不一定非要明确一次究竟跳几步，每次取最大的跳跃步数，这个就是可以跳跃的覆盖范围。

这个范围内，别管是怎么跳的，反正一定可以跳过来。

**那么这个问题就转化为跳跃覆盖范围究竟可不可以覆盖到终点！**

**贪心算法局部最优解：每次取最大跳跃步数（取最大覆盖范围），整体最优解：最后得到整体最大覆盖范围，看是否能到终点**。



## [45. 跳跃游戏 II](https://leetcode.cn/problems/jump-game-ii/description/)

给定一个长度为 `n` 的 **0 索引**整数数组 `nums`。初始位置为 `nums[0]`。

每个元素 `nums[i]` 表示从索引 `i` 向前跳转的最大长度。换句话说，如果你在 `nums[i]` 处，你可以跳转到任意 `nums[i + j]` 处:

- `0 <= j <= nums[i]` 
- `i + j < n`

返回到达 `nums[n - 1]` 的最小跳跃次数。生成的测试用例可以到达 `nums[n - 1]`。

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        // 长度为1
        if(nums.size() <= 1) return 0;
        // 记录当前的覆盖范围 和 下一次的覆盖范围
        int cur = 0, next = 0;
        int res = 0;
        for(int i = 0; i < nums.size(); i ++ ) {
            // 更新下次的覆盖范围 保证最大
            next = max(next, nums[i] + i);
            // 已经到当前覆盖范围的最后了
            if(i == cur) {
                // 判断是否覆盖到最后
                if(i != nums.size() - 1) {
                    // 没有覆盖到最后，启动下一次覆盖范围
                    // 同时步数 +1
                    res ++;
                    cur = next;
                    // 启动之后发现可以覆盖了
                    if(cur >= nums.size() - 1) break;   // 直接返回
                }else break;    // 已经覆盖到最后，直接退出
            }
        }
        return res;
    }
};
```

贪心的思路，局部最优：当前可移动距离尽可能多走，如果还没到终点，步数再加一。整体最优：一步尽可能多走，从而达到最小步数。

**这里需要统计两个覆盖范围，当前这一步的最大覆盖和下一步最大覆盖**。

如果移动下标达到了当前这一步的最大覆盖最远距离了，还没有到终点的话，那么就必须再走一步来增加覆盖范围，直到覆盖范围覆盖了终点。