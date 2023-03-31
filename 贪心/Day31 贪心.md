# Day31 贪心

● 理论基础 

● 455.分发饼干 

● 376. 摆动序列 

● 53. 最大子序和 

## 贪心一般解题步骤

贪心算法一般分为如下四步：

- 将问题分解为若干个子问题
- 找出适合的贪心策略
- 求解每一个子问题的最优解
- 将局部最优解堆叠成全局最优解

## [455. 分发饼干](https://leetcode.cn/problems/assign-cookies/description/)

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 `i`，都有一个胃口值 `g[i]`，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 `j`，都有一个尺寸 `s[j]` 。如果 `s[j] >= g[i]`，我们可以将这个饼干 `j` 分配给孩子 `i` ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int i = 0, j = 0, res = 0;
        while(i < g.size() && j < s.size()) {
            // s[j] >= g[i] 满足 同时移动
            // 不满足 移动 j
            while(j < s.size() && s[j] < g[i]) j ++;
            if(j < s.size() && s[j] >= g[i]) {
                i ++, j ++;
                res ++;
            }
        }
        return res;
    }
};
```

**这里的局部最优就是大饼干喂给胃口大的，充分利用饼干尺寸喂饱一个，全局最优就是喂饱尽可能多的小孩**。

## [376. 摆动序列](https://leetcode.cn/problems/wiggle-subsequence/description/)

```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int curDiff = 0, preDiff = 0;   // 当前两个数的差，前两个数的差
        int res = 1;
        for(int i = 0; i < nums.size() - 1; i ++ ) {
            // 计算当前差
            curDiff = nums[i + 1] - nums[i];
            // 之所以是 preDif = 0，可以在平坡的时候删除前面的，留下最后一个
            if((preDiff >= 0 && curDiff < 0) || (preDiff <= 0 && curDiff > 0)) {
                res ++;
                // 放在里面是因为只需要在改变的时候更新
                // 避免上坡出现平坡
                preDiff = curDiff;
            }
        }
        return res;
    }
};
```

**本题异常情况的本质，就是要考虑平坡**， 平坡分两种，一个是 上下中间有平坡，一个是单调有平坡

**局部最优：删除单调坡度上的节点（不包括单调坡度两端的节点），那么这个坡度就可以有两个局部峰值**。

**整体最优：整个序列有最多的局部峰值，从而达到最长摆动序列**。

## [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/description/)

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if(nums.size() == 0 ) return 0;
        int maxv = nums[0];
        // 思路就是如果当前值加上前面的最大值大于当前值，就将当前值改为这个大的
        for(int i = 1; i < nums.size(); i ++ ) {
            nums[i] = max(nums[i] + nums[i-1], nums[i]);
            maxv = max(maxv, nums[i]);
        }
        return maxv;
    }
};
```

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if(nums.size() == 0) return 0;
        // 遇到负数肯定会拉低整体值 
        int res = nums[0];
        int sum = 0;
        for(int i = 0; i < nums.size(); i ++ ) {
            sum += nums[i];
            res = max(res, sum);
            if(sum <= 0) sum = 0;
        }
        return res;
    }
};
```

局部最优：当前“连续和”为负数的时候立刻放弃，从下一个元素重新计算“连续和”，因为负数加上下一个元素 “连续和”只会越来越小。

全局最优：选取最大“连续和”

**局部最优的情况下，并记录最大的“连续和”，可以推出全局最优**。